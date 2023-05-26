+++
title = "FreeBSDの「su -」が使えない"
description = ""
date = 2023-05-26
[extra]
[taxonomies]
categories = ["tech"]
tags = ["FreeBSD"]
+++

# /usr/local/etc/sudoers

をお好きなエディタで開いて

```sh
# %wheel ALL=(ALL) ALL
```

をアンコメントすればいい。

あ、もちろんwheelグループに入ってる状態やで。

## visudoじゃだめなん

- なんかぶっ壊れとる（保存できない）

[FreeBSD/sudoのman](https://man.freebsd.org/cgi/man.cgi?query=sudoers)


