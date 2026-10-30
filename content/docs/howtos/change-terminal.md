---
title: "Change the Default Terminal"
linkTitle: "Change the Default Terminal"
description: >
  Change which terminal Regolith loads by default
---

The terminal keybinding, {{< keys "super,enter" >}}, runs `x-terminal-emulator`, which is `gnome-terminal` by default. Any terminal emulator can be used instead. Both methods below change the terminal of the X11 (i3) and the Wayland (sway) session alike. A Wayland-only terminal such as `foot` does not start in the X11 session, so choose one that runs in both unless you only use the Wayland session.

## For all users

To update Regolith to use your already installed terminal of your choice, use `update-alternatives` to specify it as default:

1. Run `sudo update-alternatives --config x-terminal-emulator` to see all available terminals installed on your system.
2. Type the number of the terminal you prefer and press Enter.

## For your user only

Set the Xresource `wm.program.terminal` to the terminal command, then refresh the session. For example, for `alacritty`:

```bash
echo "wm.program.terminal: /usr/bin/alacritty" >> ~/.config/regolith3/Xresources
regolith-look refresh
```
