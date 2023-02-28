+++
title = "SpotifydのKeyringについて"
description = "３時間格闘してた"
date = 2022-07-27
[extra]
[taxonomies]
categories = ["tech"]
tags = ["ArchLinux","GNU/Linux","Spotifyd"]
+++

# パスワードをconfに直接貼りたくない
そう思ってGnome keyringを使おうとしたら使えなかったので回避策(?)を載せます

# 結論
```bash
$ secret-tool store --label='name you choose' application rust-keyring service spotifyd username <your-username>
```
`name your choose`は自分でお好きなように. `<your-username>`はSpotifydのユーザーネームを入れてください.

# 参考文献
- https://crates.io/crates/spotifyd/0.3.0#configuration

