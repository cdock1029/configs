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

compositor, 30 bit
https://bugs.kde.org/show_bug.cgi?id=406302#c8

# IMPORTANT
https://bugs.kde.org/show_bug.cgi?id=442386

> JFYI, I have to add 
> `export QT_XCB_GL_INTEGRATION=xcb_egl`
> to `kwin.sh` to make composing work (arch, intel with modesetting, x11, plasma 5.26.5 )

Also
```
Again a short update. During the last few weeks it has been shown that the best way to enforce EGL in kwin is through the "KWIN_OPENGL_INTERFACE=EGL" and not the "KWIN_COMPOSE=O2ES" variable.

This will effectively keep the OpenGL 2.0 or OpenGL 3.1 "function feature set" and just force EGL over GLX.

sudo nano /etc/profile.d/kwin.sh
export KWIN_OPENGL_INTERFACE=EGL
```
#### KWIN/Plasma/KDE

https://wiki.archlinux.org/title/Uniform_look_for_Qt_and_GTK_applications#Consistent_file_dialog_under_KDE_Plasma
- `export PLASMA_USE_QT_SCALING=1`
- `.config/plasma-workspace/env/kwin.sh`
  - ```bash
    #!/bin/sh

    export QT_XCB_GL_INTEGRATION=xcb_egl
    export KWIN_OPENGL_INTERFACE=egl
    export KWIN_TRIPLE_BUFFER=1
    export KWIN_USE_INTEL_SWAP_EVENT=1
    ```
- see Firefox egl settings
