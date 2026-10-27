---
title: "Change the Terminal Font Size"
linkTitle: "Change the Terminal Font Size"
description: >
  Change the font used in the terminal
---

Regolith sets the font of its GNOME Terminal profile, named "Regolith", from the `gnome.terminal.font` `Xresources` key. By default, this is the monospace font of the current look. The steps are the same in the X11 (i3) and Wayland (sway) sessions.

1. Look up the current value with `xrescat gnome.terminal.font` in the X11 session, or `trawlcat gnome.terminal.font` in the Wayland session. With the default look, it is `BitstreamVeraSansMono Nerd Font 12`.

2. Create or add the following value to your `~/.config/regolith3/Xresources` file, with the font name and the size you want. For example, the default font in size 14:

```yaml {filename="~/.config/regolith3/Xresources"}
gnome.terminal.font: BitstreamVeraSansMono Nerd Font 14
```

To use another font, install it first. For example, after `sudo apt install fonts-jetbrains-mono` you can use `JetBrains Mono 14`. `fc-list : family` lists the names of the installed fonts.

3. Reload the `Xresources` configuration:

```bash
regolith-look refresh
```

The Regolith profile is updated, and open GNOME Terminal windows change to the new font.

This only applies to GNOME Terminal. If you [use another terminal]({{< ref "change-terminal" >}}), set the font in that terminal's own configuration. To stop Regolith from changing the GNOME Terminal profile at all, set `gnome.terminal.update: false`.
