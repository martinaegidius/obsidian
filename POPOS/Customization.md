Re-did theming - inconsistency between gnome and nautilus elements was caused because pop-shell needs "user themes" extensions enabled. 
Theme details: 
- Using nord-darker 40 with tela orange icon-set. 
- Chrome needs to inherit gtk-theme, but this does not function properly. Firefox inherits fine. 
- Trying to make flatpak apps inherit GTK-themes: 
	- did, did not work
	- Added nord theme to obsidian

- Added blur-my-shell to application window headers, nautilus, bash shell, overview and panel 

```
sudo flatpak override --filesystem=$HOME/.themes
sudo flatpak override --filesystem=$HOME/.icons

```
sudo flatpak override --env=GTK_THEME=my-theme 
sudo flatpak override --env=ICON_THEME=my-icon-theme 
```

Trying to add blurred shell hoping it works this time 
