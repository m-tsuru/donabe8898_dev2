+++
title = "Nvidia+X11でフレームレートが出ない問題"
description = ""
date = 2023-02-28
[extra]
[taxonomies]
categories = ["tech"]
tags = ["ArchLinux","GNU/Linux","nvidia","KDE", "X window system"]
+++

# 環境

|OS|Graphics|GUI|
|:--|:--|:--|
|Arch Linux 6.2.1 zen kernel|nvidia Geforce GTX 1660ti (NV168)|Plasma X11 session|

接続しているモニター

- AOCのよく分からんげーみんぐもにたぁ
    - 型番: 24G1WG4
    - リフレッシュレート: 144 Hz
- LGのさぶもにたぁ
    - 型番: 22MN430M
    - リフレッシュレート: 75 Hz

# 経緯

Plasma Wayland sessionを使っていたが, 5.27.1にアップデートしたら突然ご臨終したのでX11 sessionに移行した。
その際, 全モニターのフレームレートが一番低いやつ（漏れの環境なら75 Hz）に合わせられることに気がついた.


# 解決策

`/etc/environment`に以下を追加する

```.conf
CLUTTER_DEFAULT_FPS=144
__GL_SYNC_DISPLAY_DEVICE=DP-0
__GL_SYNC_TO_VBLANK=0
KWIN_X11_NO_SYNC_TO_VBLANK=1
KWIN_X11_REFRESH_RATE=240000
KWIN_X11_FORCE_SOFTWARE_VSYNC=1
```

# 備考

- `__GL_SYNC_DISPLAY_DEVICE=`にリフレッシュレートを合わせたいモニターを記述する. モニター名はnvidia xserver settingから見れる

    - ArchLinuxならpacmanでｲﾝｽｺ可  __[nvidia-settings](https://archlinux.org/packages/extra/x86_64/nvidia-settings/)__

- `nvidia xserver setting`からも設定できるらしいが保存しても適用されなかったのでこれが一番かも.

- GNOMEだとKWINの項目はいらないが解決しない場合があるとかないとか.

