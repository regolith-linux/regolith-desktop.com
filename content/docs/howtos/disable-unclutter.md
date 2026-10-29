---
title: "Disable Mouse Hiding (unclutter)"
linkTitle: "Disable Mouse Hiding (unclutter)"
description: >
  Stop the mouse cursor from hiding
---

By default the mouse cursor disappears if unused for a few seconds. If you prefer that the mouse cursor always remains visible on the screen, follow the steps for your session.

## X11 session

In the X11 session (i3), `unclutter-xfixes` hides the cursor.

1. Specify an empty value for the unclutter launcher program with an Xresources override:
  ```bash
  echo "wm.program.unclutter: :" >> ~/.config/regolith3/Xresources
  ```
2. Log out and back in for changes to take effect.

Alternatively, remove the package that starts it with `sudo apt remove regolith-i3-unclutter`, then log out and back in.

## Wayland session

In the Wayland session, sway hides the cursor after 5 seconds without movement, and also while you type. To turn off both:

1. Add these Xresources overrides (a timeout of `0` turns off hiding):
  ```bash
  echo "wm.hide_cursor.timout: 0" >> ~/.config/regolith3/Xresources
  echo "wm.hide_cursor.show_when_typing: disable" >> ~/.config/regolith3/Xresources
  ```
2. Run `regolith-look refresh`.

Alternatively, remove the package that sets this up with `sudo apt remove regolith-sway-unclutter`, then log out and back in.

The key is spelled `timout`, as in the Regolith configuration. Set `wm.hide_cursor.timout` to another value in milliseconds to change the delay instead.
