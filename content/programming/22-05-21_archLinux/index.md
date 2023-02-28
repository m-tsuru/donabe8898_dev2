+++
title = "ArchLinuxインストールしようとしrEFIndでハマった"
description = "底なし沼だこれ"
date = 2022-05-21
[extra]
[taxonomies]
categories = ["tech"]
tags = ["ArchLinux","OS","GNU/Linux"]
+++

# WindowsPCにArchLinuxをインストールしてrEFIndでデュアルブート
先日、N700Sのグリーン車に乗っていたら急にArchLinuxを使いたくなったので自分のＰＣにインストールしました。
<img src="n700s_green.jpg" alt="n700sG" width="50%">
めちゃくちゃ快適なので新幹線のグリーン車乗るのやめられない症候群になっちゃってます。

---

# 公式ドキュメント見ればインストールはできる。インストールは。
それはさておき、↑これはマジです。実際、インストールしてrEFIndの壁にぶち当たるまでは公式ドキュメントと、同じことをしているお二方のブログ等を見ただけです。

- [公式ドキュメント](https://wiki.archlinux.jp/index.php/インストールガイド)

- [Arch LinuxとWindows 10をデュアルブートした話](https://ain0204.hatenablog.com/entry/2016/07/29/032122)

- [Arch Linuxのすゝめ](https://qiita.com/YTJVDCM/items/eea145c59662adb8ae2a)

---
# そして立ちはだかる壁
僕がこの記事を書こうと思ったただ一つの理由、それがブートローダに使った __rEFInd__ です。と言いますのも、OSのインストールが完了していざreboot！と思ったらなにやら様子がおかしい...

![1](pic1.jpg)

この状態ではsudoやpacmanといったコマンドが使用できなかったため失敗かと思い再インストール...したのですがやっぱり同じ現象になる...

という訳でTwitterであれこれ言ってたらフォロワーの方から返信が


![rep](rep1.png)

...ほほう...要するにブート時に指定するルートディレクトリを変更すればいいんだな？んじゃさっそくrefind_linux.conf覗いてみるお

![pic2](pic2.jpg)

僕「なるほど、わからん」

仕方がないのでまたフォロワー様に泣きついたところ

![rep2](rep2.png)

僕「なるほどない、分かった」

なんと資料まで添付して送ってくれました。これを見ると、
`hoge`の部分を自分のコンピュータで`blkid`を打ち込んだときのPARTUUIDに設定し打ち込めば...

__起動しました！__ という訳で、こんな僕でもArchLinuxをインストールすることが出来ました...救いの手を差し伸べてくれた[女神様](https://twitter.com/yude_jp)、ありがとうございました。

参考
- [refind_linux.confに記述したもの](https://pastebin.com/Vfui4FzJ)
- [yudeさん](https://twitter.com/yude_jp)
