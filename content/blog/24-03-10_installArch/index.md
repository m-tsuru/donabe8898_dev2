+++
title = "またArchLinuxをインストールしようとしてる人へ"
description = "systemdは特に気をつける"
date = 2024-03-10
[taxonomies]
tags = ["ArchLinux","OS","GNU/Linux"]
+++

ArchLinuxはシンプルなOSで非常に魅力的な魅力を持っている。

> __A simple, lightweight distribution__ \
> \
> You've reached the website for Arch Linux, a lightweight and flexible Linux® distribution that tries to Keep It Simple.


和訳
> __シンプル、軽量なディストリビューション__ \
> \
> Arch Linux のウェブサイトにようこそ。Keep It Simple を標榜する、軽量で柔軟性に優れた Linux® ディストリビューションです。

 - [https://https://archlinux.org/]("https://https://archlinux.org/")

## メリット
- シンプルなので壊れにくい（~~ト○タの自動車みたいな図太さ~~）
- ローリング・リリースで最新のソフトウェアを扱うことができる
- 自由（自由が大事ってそれ一番言われてるから by [RMS](https://www.gnu.org/philosophy/free-sw.html) ）

# 1. ISO焼き

1. ArchのISOを落とす
    - [筑波大学のミラー: http://ftp.tsukuba.wide.ad.jp/Linux/archlinux/iso/2024.03.01/]("http://ftp.tsukuba.wide.ad.jp/Linux/archlinux/iso/2024.03.01/")

2. USBメモリかDVD/BDに焼く

## Windows
- Rufusを使うのが一般的

## Linux
```bash
sudo dd bs=4M if=archlinux.iso of=/dev/sdX conv=fsync oflag=direct status=progress
```
- `dd` - ドライブを焼くために使うコマンド. ドライブのベンチマークにも使えるらしい.
- `bs=4M` - 一回の書き込み処理でどれくらいの量を書き込むか. これをつけないとバカ遅くなる.
- `if=` - isoファイルを指定. ディレクトリで指定することもできる.
    - /home/user/Download/archlinux.iso
- `of=` - 書き込み先のドライブ. 必ず`lsblk`コマンドで確認すること.

# 2. 起動からのパーティション割

1. インストールディスクをPCに突っ込んで起動
    - __ノートPCは爆音でスーパーマ○オのコイン獲得音が鳴るので注意__

2. `lsblk`コマンドでインストール先のディスクを確認

3. `ping`コマンドでネットワークの疎通確認（有線LANで接続したほうがやりやすい. wifiも使えるが設定あり: [https://wiki.archlinux.jp/index.php/Iwd]("https://wiki.archlinux.jp/index.php/Iwd")）

4. `cgdisk /dev/DRIVE_NAME`でパーティション分割

- UEFI+GPTでNVMeSSDにインストールする場合（最近のPCはコレ）

| パーティション | 割当サイズ | 役割 |
|:--:|:--:|:--:|
| /dev/nvme0n1p1 | 1GB | ブートローダとかカーネルイメージの格納 |
| /dev/nvme0n1p2 | メモリ量の倍 | スワップ領域 |
| /dev/nvme0n1p3| 残りすべて | root |

5. ファイルシステム作成

```bash
mkfs.fat -F 32 /dev/nvme0n1p1 # FAT32でフォーマット
mkfs.ext4 /dev/nvme0n1p3 # Linuxを日常で使うならext4でOK
mkswap /dev/nvme0n1p2 # swap領域に指定
```

6. ファイルシステムのマウント
- いま起動しているarchlinuxがドライブを認識&中身にアクセスできるようにマウントさせる

```bash
mount /dev/nvme0n1p3 /mnt # インストール先のrootを起動しているarchlinuxのrootにマウント
mount --mkdir /dev/nvme0n1p1 /mnt/boot # インストール先のドライブにbootディレクトリを作って起動しているarchlinuxのbootにマウント
swapon /dev/nvme0n1p2 # swap領域として設定
```

# 3. インストール
- ミラーの指定とかめんどいのでサクッとやっちゃう（~~最小環境なら大した時間差じゃねぇし~~）

```bash
pacstrap /mnt base base-devel linux linux-firmware linux-headers nano grub efibootmgr networkmanager
```
- `pacstrap` - 指定したディレクトリにソフトウェアをインストールするだけのコマンド
- `base` - linuxカーネルを扱うために必要なソフトウェア群. 所謂ユーザーランド. ほとんどGNU Coreutils
- `base-devel` - プログラミングに必要な基本のソフトウェア. 一見不必要だけど入れておいて損はない.
- `linux` - ArchLinux公式のLinuxカーネル. x86_64に最適化されてて無駄なパッチはあたっていないので基本はこれでOK
- `linux-firmware` - ファームウェア. 特にwifi&bluetoothモジュールはコレがないと動かない場合が多い.
- `linux-headers` - linuxカーネルのヘッダファイル. 一見不必要だけど入れておいて損はない.
- `nano` - GNU Nano　テキストエディタ. 初心者にやさしい.
- `grub` - ブートローダー. 一番無難.
- `efibootmgr` - UEFIマザーのPCなら必須.
- `networkmanager` - ネットワークに接続するためのやつ. DHCP機能付き. デスクトップ環境とシームレスに接続できる.

# 4. 色々と設定する

1. fstabの生成
- Linuxが起動時にrootドライブやスワップ領域を認識できるように, PCのHDDやSSDのUUID等が記録されたファイルを生成する

```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

2. chroot環境に入る
- 先程インストールした環境に入って作業する
```bash
arch-chroot /mnt
```

3. タイムゾーンの設定
```
ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
```
- OSがどの地域の日付時間を使うのかを設定する

4. ハードウェアクロック（PCの時計）の書き込み
```bash
hwclock --systohc
```
- windowsを含むマルチブートの場合, ちょっとした工夫が必要（片方の時間が狂う狂う狂う狂う）

5. ロケールの設定
- `/etc/locale.gen`をエディタで開いて
```bash
en_US.UTF-8 UTF-8
ja_JP.UTF-8 UTF-8
```
の行をコメントアウトするか追記する

6. ロケール生成
```bash
locale-gen
```
システムの扱う言語を設定する（この時点では英語と日本語が使えるように候補を決めてる）

```bash
echo LANG=ja_JP.UTF-8 > /etc/locale.conf
echo KEYMAP=jp106 > /etc/vconsole.conf
```
- `echo KEYMAP=jp106 > /etc/vconsole.conf`は日本語キーボード使用時に役立つ

7. ホスト名の決定

ここから先はまたこんど書く
gitignoreに記載済み
