# Linux / OpenSUSE Configuration

## KDE

### Display resolution

- For `3840x2400` X1 Carbon
  - Display and Monitor Global Scale: 168.75%
 
#### [Integer Scaling, forget about fractional scaling](https://tonsky.me/blog/monitors/)
- Strong case for integer (200%) scaling. Aligns to pixels accurately, and scales icons better on KDE


### Fonts

- DPI: 162 (Automatically set after changing Global scale)
- Fonts:
  - General: Noto Sans 11pt
  - Fixed: JetBrainsMono 10pt
  - Small: Noto Sans 9pt
  - Toolbar: Noto Sans 10pt
  - Menu: Noto Sans 11pt
  - Window title: Noto Sans 11pt
 
  #### Possible use of OTF for better rendering
    https://www.reddit.com/r/kde/comments/kzf6gx/best_fonts_and_rendering_settingsoptions/gkjpb86/

    Cantarell OTF
    
    https://github.com/tonsky/FiraCode/issues/1233#issuecomment-1448651984

    Config for fixing TTF? In `/etc/environment`

    `FREETYPE_PROPERTIES="cff:no-stem-darkening=0 autofitter:no-stem-darkening=0"`

    https://www.reddit.com/r/elementaryos/comments/tpvxed/do_you_feel_like_the_default_font_inter_is_too/
  
  #### Hingting and fontconfig
  https://venam.net/blog/unix/2020/09/14/playing_with_fonts.html

  https://wiki.archlinux.org/title/font_configuration#Applications_overriding_hinting
  `~/.config/kdeglobals` overrides settings for Firefox etc
  ```
  XftAntialias=true
  XftHintStyle=hintfull
  XftSubPixel=none
  ```
  - Flatpak special config dir:
    - `~/.var/app/org.mozilla.firefox/config/fontconfig/fonts.conf`
    - override a font:
    ```xml
    <?xml version="1.0"?>
    <!DOCTYPE fontconfig SYSTEM "fonts.dtd">
    <fontconfig>    
      <match>
        <test name="family"><string>Liberation Mono</string></test>
          <edit name="family" mode="assign" binding="strong">
            <string>JetBrains Mono</string>
          </edit>
      </match>
    </fontconfig>
    ```

### Settings

- System Bell off: `Accessibility -> Audible Bell -> uncheck Enable`
- Splash Screen: None
- SDDM Login Screen: change picture, Apply Plasma Settings
- Screen Locking: `Screen Locking -> Appearance -> Configure`
- Workspace Behavior -> General Behavior -> Animation Speed, inc 1
  - Click files selects them
- Input Devices
  - Keyboard: Delay 356ms
 
### Zypper / Yast
- Remove and lock packages (`sudo zypper al`) or mark taboo in Yast:
  - akonadi-contact
  - discover
  - games (pattern)
  - kde_games (pattern)
  - kde_office (pattern)
  - office (pattern)
  - libdvdcss2
  - plasma5-pk-updates
  - xorg-x11-fonts
  - xorg-x11-fonts-legacy
  - MozillaFirefox (if using flatpak. this was randomly re-installed)
 
  - [Undoing/removing pacman vendor change and removing pacman packages](https://forums.opensuse.org/t/undoing-removing-pacman-vendor-change-and-removing-pacman-packages/143223/10)

### Telegram

flatpak QT environment variable to fix rendering resolution:

    flatpak override --env=QT_SCALE_FACTOR_ROUNDING_POLICY=Round org.telegram.desktop

### Environment
- use KDE file chooser `export GTK_USE_PORTAL=1`
- [KDE Developer setup](https://community.kde.org/Get_Involved/development/Set_up_a_development_environment#Disable_indexing_for_your_development_environment): Disable indexing for your development environment

> You'll want to disable indexing for your development-related git repos and the files they will build and install. Add the directory ~/kde to the exclusions list in System Settings > Workspace > Search > File Search

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



## Progamming

### Git

- git libsecret credential helper to save auth
  - git-credential-libsecret
  - libsecret-devel
  -     git config --global credential.helper /usr/libexec/git/git-credential-libsecret
  -     git config --global credential.helper /usr/libexec/git/git-credential-libsecret

### Qt

#### Creator Settings

- `clangd` avoid header include instead of class/module include

  - `C++ -> Clangd -> uncheck "insert header files on completion`

- code style Webkit
  - `Beautifier -> Clang Format -> Use predefined style Webkit`

## Browser

### Brave

- hardware acceleration [Flags](https://bbs.archlinux.org/viewtopic.php?id=244031&p=33)
  `--ignore-gpu-blocklist --enable-features=VaapiVideoDecoder,VaapiVideoEncoder,VaapiVideoDecodeLinuxGL,VaapiIgnoreDriverChecks --disable-features=UseChromeOSDirectVideoDecoder,UseSkiaRenderer`

  Pass flags to `flatpak`:
  https://github.com/flatpak/flatpak/issues/2913#issuecomment-944976842

  Like this below, but desktop files are in /var:
  `/var/lib/flatpak/exports/share/applications/com.brave.Browser.desktop`

  > So in summary what I ended up doing was following @JKAnderson409 's advice and did this:
  > 
  > ```shell
  > cp ~/.local/share/flatpak/exports/share/applications/org.signal.Signal.desktop \
  >    ~/.local/share/applications/
  > 
  > # Edit copy to my liking without modifying the original.
  > vim ~/.local/share/applications/org.signal.Signal.desktop 
  > ```
  > 
  > My custom `~/.local/share/applications/org.signal.Signal.desktop` now looks like this:
  > 
  > ```shell
  > [Desktop Entry]
  > Name=Signal
  > Exec=/usr/bin/flatpak run --branch=stable --arch=x86_64 --command=signal --file-forwarding org.signal.Signal --enable-features=UseOzonePlatform --ozone-platform=wayland  @@u %U @@
  > 
  > # ...more stuff below this line, which I did not modify
  > ```

### Firefox
- Install Tab Center Reborn extension
- #### [How to move sidebar to the right](https://www.simplehelp.net/2008/09/04/how-to-move-your-sidebar-to-the-right-side-of-firefox/)
- Move sidebar to Right
  - Make sure `#sidebar-header` visibility is not collapsed in userChrome.css
  - click on Bookmarks drop down inside sidebar header

- #### [User Chrome instructions](https://www.reddit.com/r/FirefoxCSS/comments/73dvty/tutorial_how_to_create_and_livedebug_userchromecss/)
  - Windows app store Firefox location: `C:\Users\ms\AppData\Local\Packages\Mozilla.Firefox_abcdefghij\LocalCache\Roaming\Mozilla\Firefox\Profiles\1pxxyyzz.default-release\chrome\userContent.css`

  #### firefox smooth scrolling
  - Add to `.profile`
  `export MOZ_USE_XINPUT2=1`
  
  #### `about:config`

  ##### GPU (not sure what's necessary now)

  - layers.gpu-process.enabled true Lowers dropped frames on Youtube
  - gfx.webrender.enabled true
  - gfx.webrender.all true
  - gfx.webrender.compositor true
  - gfx.webrender.compositor.force-enabled
 
  #### Flatpak
  - Add: `google-noto-sans-cjk-fonts`
  - Remove: `sudo zypper remove xorg-x11-fonts xorg-x11-fonts-legacy` and add lock to prevent re-install: `sudo zypper al xorg-x11-fonts xorg-x11-fonts-legacy`
  - Configuration that might be needed for apps, in `.profile`: `export XDG_DATA_DIRS="/var/lib/flatpak/exports/share:/home/cdock/.local/share/flatpak/exports/share:$XDG_DATA_DIRS"`
  - profile location:
      Everything that is normally stored in $HOME is in `~/.var/app/org.mozilla.firefox/`. You want the "Root Directory":
      `~/.var/app/org.mozilla.firefox/.mozilla/firefox/[profile name]`

  - MPV Config:
    `.var/app/io.mpv.Mpv/config/mpv/mpv.conf `
    
    ```
    profile=gpu-hq
    vo=gpu
    hwdec=auto
    ```

### NextDNS
https://github.com/yokoffing/NextDNS-Config#blocklists-1
  
