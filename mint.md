### Ptyxis terminal transparency

```shell
flatpak run --command=/bin/bash app.devsuite.Ptyxis
gsettings get org.gnome.Ptyxis default-profile-uuid
gsettings set org.gnome.Ptyxis.Profile:/org/gnome/Ptyxis/Profiles/${ID_VALUE_FROM_ABOVE}/ opacity 0.8
```

https://gitlab.gnome.org/chergert/ptyxis/-/issues/99#note_2228092
