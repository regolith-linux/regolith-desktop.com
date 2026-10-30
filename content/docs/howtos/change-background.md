---
title: 'Change background'
linkTitle: 'Change background'
description: >
    Change the background
---

## Wallpaper

Most Regolith Looks provide a default desktop color or image. The wallpaper can be set by specifying the path to the wallpaper image via the Xresources key `regolith.wallpaper.file`. The keys on this page work in both the X11 (i3) and the Wayland (sway) session (Debian 12 has the X11 session only), with the differences listed in [Wayland (sway)](#wayland).

You can also pick a wallpaper under **Background** in the Appearance page of the Regolith Control Center, which can be opened by running `regolith-control-center background` in the terminal. What it does depends on the release. On Ubuntu 24.04 and Debian 12 and 13, it writes `regolith.wallpaper.file` and `regolith.lockscreen.wallpaper.file` to `~/.config/regolith3/Xresources` and refreshes, so the choice stays after `regolith-look refresh`, also sets the lock screen, and applies in sway too; remove those lines to undo it. On Ubuntu 25.10 and 26.04, it changes only the X11 wallpaper, until the next `regolith-look refresh` applies the Xresources values again. The Regolith Control Center on Ubuntu 22.04 has no Background panel.

Example, assuming the file `/usr/share/backgrounds/warty-final-ubuntu.png` is present (it comes with the `ubuntu-wallpapers` package; on Debian, use `/usr/share/images/desktop-base/default` from the `desktop-base` package):

```bash
echo "regolith.wallpaper.file: /usr/share/backgrounds/warty-final-ubuntu.png" >> ~/.config/regolith3/Xresources
regolith-look refresh
```

In the above command the property mentioned is set to the `/usr/share/backgrounds/warty-final-ubuntu.png` change this path to any other that needs to be used.

To change the image treatment (zoom, scale, etc.) of the wallpaper image in the X11 session, set `regolith.wallpaper.options`. Options are: `none`, `wallpaper`, `centered`, `scaled`, `stretched`, `zoom`, `spanned`. The default is `wallpaper` (tiled).

Example:

```bash
echo "regolith.wallpaper.options: zoom" >> ~/.config/regolith3/Xresources
regolith-look refresh
```

To specify a color rather than an image use the Xresource key `regolith.wallpaper.color.primary`. Use the `#RRGGBB` form, which both sessions understand. Most looks also set a secondary color for a gradient, so clear it to get a solid color:

```bash
echo "regolith.wallpaper.file: " >> ~/.config/regolith3/Xresources
echo "regolith.wallpaper.color.primary: #0000FF" >> ~/.config/regolith3/Xresources
echo "regolith.wallpaper.color.secondary: " >> ~/.config/regolith3/Xresources
regolith-look refresh
```

To specify a secondary color and gradient (X11 session only; `regolith.wallpaper.color.shading.type` is `horizontal` or `vertical`):

```bash
echo "regolith.wallpaper.file: " >> ~/.config/regolith3/Xresources
echo "regolith.wallpaper.color.primary: #0000FF" >> ~/.config/regolith3/Xresources
echo "regolith.wallpaper.color.secondary: #00FF00" >> ~/.config/regolith3/Xresources
echo "regolith.wallpaper.color.shading.type: vertical" >> ~/.config/regolith3/Xresources
regolith-look refresh
```

### Lockscreen Wallpaper

The lockscreen wallpaper can be managed in the same way as the desktop wallpaper. The keys are the same as desktop wallpaper, but with `lockscreen` prefixed to `wallpaper`, like so: `regolith.lockscreen.wallpaper...`. For example, to specify a lockscreen image:

```bash
echo "regolith.lockscreen.wallpaper.file: /usr/share/backgrounds/warty-final-ubuntu.png" >> ~/.config/regolith3/Xresources
regolith-look refresh
```

### Disable Wallpaper handling

If you wish to manage wallpaper externally to Regolith in the X11 session, simply specify empty values for wallpaper image and color:

```bash
echo "regolith.wallpaper.file: " >> ~/.config/regolith3/Xresources
echo "regolith.wallpaper.color.primary: " >> ~/.config/regolith3/Xresources
```

In the Wayland session this leaves a black background instead; see below.

## Wayland (sway) {#wayland}

Debian 12 has no Wayland session, so this section applies to the other releases. In sway the wallpaper is drawn by `swaybg`, which Regolith starts from `regolith.wallpaper.file` and `regolith.wallpaper.color.primary` each time the session starts or `regolith-look refresh` runs. The differences from X11 are:

- **Image placement:** `regolith.wallpaper.options` is not used. Set `regolith.wallpaper.mode` instead, to one of `stretch`, `fill`, `fit`, `center` or `tile`. The default is `fill`.

  ```bash
  echo "regolith.wallpaper.mode: fit" >> ~/.config/regolith3/Xresources
  regolith-look refresh
  ```

- **Colors:** `regolith.wallpaper.color.primary` must be in the `#RRGGBB` form. sway rejects color names such as `blue`, and no wallpaper is drawn, even when an image is set (sway shows its default background: black on newer sway, dark grey on Ubuntu 22.04 and 24.04). The secondary color and the shading type are not used, so the color is always solid.
- **Lock screen:** the lock screen (`gtklock`) shows `regolith.lockscreen.wallpaper.file`. The lockscreen color keys are not used.
- **Settings:** on Ubuntu 25.10 and 26.04, picking a wallpaper under **Background** in the Appearance page of the Regolith Control Center does not change the sway wallpaper. Use the Xresources keys.
- **Managing the wallpaper yourself:** empty values give a black background, because Regolith still sets one. Remove the package that sets it, then set the wallpaper in your own sway config partial:

  ```bash
  sudo apt remove regolith-sway-background
  mkdir -p ~/.config/regolith3/sway/config.d
  echo "output * bg /usr/share/backgrounds/warty-final-ubuntu.png fill" > ~/.config/regolith3/sway/config.d/90_wallpaper
  swaymsg reload
  ```
