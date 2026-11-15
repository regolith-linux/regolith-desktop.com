---
title: "Disable Mouse Hiding (unclutter)"
linkTitle: "Disable Mouse Hiding (unclutter)"
description: >
  Stop the mouse cursor from hiding
---

By default the mouse cursor disappears as soon as you start typing, and when it is unused for a few seconds. It comes back when you move the mouse, press a button or turn the wheel. If you prefer that the mouse cursor always remains visible on the screen, follow the steps below.

## Both sessions

Both sessions read the same two Xresources. A timeout of `0` turns off the hiding of an unused cursor, and `disable` keeps the cursor visible while you type. To turn off both:

1. Add these Xresources overrides:
  ```bash
  echo "wm.hide_cursor.timeout: 0" >> ~/.config/regolith3/Xresources
  echo "wm.hide_cursor.show_when_typing: disable" >> ~/.config/regolith3/Xresources
  ```
2. Run `regolith-look refresh`, which reloads the Xresources and restarts i3 or reloads sway.

Keep only one of the two lines to turn off one kind of hiding, or set `wm.hide_cursor.timeout` to another value in milliseconds to change the delay instead.

regolith-wm-config 4.11.12 and earlier read only the misspelled `wm.hide_cursor.timout`. Newer versions read both spellings, and `timeout` wins. In regolith-wm-config 4.13.0 and earlier, the X11 session ignores both keys: there, use the X11 steps below.

## X11 session

In the X11 session (i3), you can also stop i3 from starting any cursor hiding:

1. Specify an empty value for the unclutter launcher program with an Xresources override:
  ```bash
  echo "wm.program.unclutter: :" >> ~/.config/regolith3/Xresources
  ```
2. Log out and back in for changes to take effect.

`wm.program.unclutter` can also name another program to hide the cursor, such as `/usr/bin/unclutter -b`.

Alternatively, remove the package that sets up the hiding with `sudo apt remove regolith-i3-unclutter`, then log out and back in.

## Wayland session

In the Wayland session, sway does the hiding. Instead of the Xresources above, you can remove the package that sets it up with `sudo apt remove regolith-sway-unclutter`, then log out and back in.
