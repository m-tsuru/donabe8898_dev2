+++
title = "waylandセッションのdiscordで画面共有する"
description = ""
date = 2022-11-06
[extra]
[taxonomies]
categories = ["tech"]
tags = ["ArchLinux","OBS","wayland","GNU/Linux"]
+++

# 材料
- waylandセッションで起動しているLinuxディストリビューション　１つ
    - 僕はArchLinuxを使って説明しますが、適宜読み替えてもらって大丈夫です。特にパッケージのインストールとか。

- OBS Studio　１つ

- Discord　１つ
    - canaryでもなんでも

# 作り方

1. v4l2loopback-dkmsをインストールします。linux-headersがなければそれもインストール。

### Debian GNU/Linux, Ubuntu, Linux Mint etc...

```bash
sudo apt install v4l2loopback
sudo apt install linux-headers
```

### Fedora, RHEL, Rocky Linux etc...

```bash
sudo dnf install v4l2loopback
sudo dnf install kernel-devel
```
### ArchLinux, EndeavourOS, Manjaro etc...

```bash
sudo pacman -S v4l2loopback-dkms
sudo pacman -S linux-headers
```

2. インストールした時点でカーネルモジュールにロードされていない場合があるのでロードします

```bash
sudo modprobe v4l2loopback
```

3. OBSを起動します

![ALL](vscreen.png)

仮想カメラ開始というボタンが追加されてますね


# かかった費用

5万円ほど（ワイのPC）
