---
title: "Install the Sway Compositor"
linkTitle: "Install the Sway Compositor"
description: >
  Upgrade to the Wayland-based Regolith session
---

To install the wayland session, follow these steps:

1. Install the `regolith-session-sway` package and its dependencies, which include Regolith's build of Sway (`sway-regolith`):

   ```bash
   sudo apt install regolith-session-sway
   ```

2. Log out, or reboot.
3. Select the "Regolith / Wayland" session in the login panel. In GDM, select your user and choose the session from the gear button in the lower-right corner.

   {{< img src="gdm-session-menu.png" alt="The GDM login screen with the session menu open from the gear button, and Regolith / Wayland selected" >}}

The X11 session ("Regolith / X11") stays installed, and you can choose either session at each login.

{{< callout type="info" >}}
Debian 12 (Bookworm) has the X11 session only. The Wayland session needs Debian 13 (Trixie) or a supported Ubuntu release.
{{< /callout >}}

## Hardware Support

Those using proprietary nvidia drivers for their graphics cards must set `regolith.sway.unsupported_gpu: true` in `~/.config/regolith3/Xresources`. This is not an issue with the open source drivers nouveau.  See more [here](https://github.com/swaywm/sway/issues/5392).

## Bar Changes

The wayland session uses [`i3status-rs`](https://github.com/greshake/i3status-rust) rather than `i3xrocks`.  `i3status-rs` provides a crisper and more resource-efficient approach to system monitoring.  However at this time the `i3xrocks` packages for status indicators are not integrated with `i3status-rs`.  To use a custom configuration with `i3status-rs`, copy the default configuration to a location of your choosing and set `wm.bar.status_config` to the path of your copy in `~/.config/regolith3/Xresources`:

```bash
mkdir -p ~/.config/regolith3/i3status-rust
cp /etc/regolith/i3status-rust/config.toml ~/.config/regolith3/i3status-rust/config.toml
echo "wm.bar.status_config: ~/.config/regolith3/i3status-rust/config.toml" >> ~/.config/regolith3/Xresources
```

Then edit the copy, and run `regolith-look refresh` or log back in. See the [upstream project documentation](https://docs.rs/i3status-rs/latest/i3status_rs/blocks/index.html) for further details.
