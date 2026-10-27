---
title: "Change the lock screen program"
linkTitle: "Change the lock screen program"
description: >
  Configure Regolith to use a lock screen other than the default one
---

The X11 session (i3) and the Wayland session (sway) lock the screen with different programs, so they are configured separately.

## X11 session

### Configure `gnome-flashback` to Disable Built-in Lockscreen

By default `gnome-flashback` handles the lock screen in the X11 session. This can be adjusted by modifying its configuration via the `gsettings` command line program:

```bash
gsettings set org.gnome.gnome-flashback screensaver false
```

`gnome-flashback` also locks the screen when the session is idle and before the computer suspends. With its lock screen disabled, these locks no longer happen; set them up yourself if you need them (for example with `xss-lock`).

### Cause Your Lock screen program to Run from i3 Keybinding

Specify the following override line in your `~/.config/regolith3/Xresources` file (substituting `your-script.sh` with the command you wish to use for the locking your screen):

{{< callout type="warning" >}}
Regolith 3.4 replaced the key `wm.program.lock` with `wm.program.lock_i3` for the X11 session and `wm.program.lock_sway` for the Wayland session. On Regolith 3.0 to 3.3, use `wm.program.lock`. Regolith 1.x and 2.x use `i3-wm.program.lock`.
{{< /callout >}}

```yaml {filename="~/.config/regolith3/Xresources"}
wm.program.lock_i3: your-script.sh
```

For example, assuming you have `i3lock` installed:

```bash
echo "wm.program.lock_i3: /usr/bin/i3lock" >> ~/.config/regolith3/Xresources
regolith-look refresh
```

The lock screen keybinding ({{< keys "super,escape" >}}) then runs `i3lock`.

## Wayland session

The Wayland session locks the screen with `gtklock`. The Xresource `wm.program.lock_sway` sets the lock command. It is used for the lock screen keybinding, after the session has been idle, before the computer suspends, and when the lid is closed. The command must return once the screen is locked, so start the locker in the background (for example `swaylock -f` or `gtklock -d`):

```bash
sudo apt install swaylock
echo "wm.program.lock_sway: swaylock -f -c 000000" >> ~/.config/regolith3/Xresources
regolith-look refresh
```

The `gsettings` command above has no effect in the Wayland session.
