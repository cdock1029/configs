### KWin

`qdbus org.kde.KWin /KWin supportInformation`

`export KWIN_OPENGL_INTERFACE="egl"`

in `/etc/profile.d/kwin.sh`
or `~/.config/plasma-workspace/env/`

`echo "export KWIN_OPENGL_INTERFACE=egl" >~/.config/plasma-workspace/env/use_egl.sh`

```sh
 #!/bin/sh

export FOO=bar

export BAR=foo 
```

[KWin Environment Variables](https://community.kde.org/KWin/Environment_Variables)

https://www.reddit.com/r/linuxquestions/comments/xryg63/does_linux_support_dcip3_or_10_bit/

> put `DefaultDepth 30` in `/etc/X11/xorg.conf.d/` (default value would be 24)

https://www.reddit.com/r/kde/comments/16gt99f/is_there_any_way_to_improve_performance_with_blur/

https://www.reddit.com/r/kde/comments/syf2gq/kde_on_wayland_how_to_set_30bit_color_ie_10bit/

https://www.reddit.com/r/kde/comments/knnuld/kwin_and_nvidia/

https://bugs.kde.org/show_bug.cgi?id=346275

