---
title: "Change Notification Systems"
linkTitle: "Change Notification Systems"
description: >
  Disable Rofication and add another notification system
---

Regolith ships with a notification system called [Rofication](https://github.com/regolith-linux/regolith-rofication). It is designed to work in the background and not use pop-ups as a way of alerting the user to a new notification. Rather, a bar item displays the current number of unread notifications and a dialog, opened with {{< keys "super,n" >}}, lists the current notifications. This page describes a way to install an alternative notification system which works in a more traditional way.

We will use [dunst](https://dunst-project.org/) as an example, because it works in both the X11 (i3) and the Wayland (sway) session. Similar steps can be used with other notification components; see [Other Notification Daemons](#other-notification-daemons) below.

1. Replace Rofication with `dunst`:

   ```bash
   sudo apt install dunst libnotify-bin
   ```

   `dunst` conflicts with Rofication, so `apt` also removes `regolith-rofication` and `regolith-wm-rofication-ilia`, the package that starts Rofication and binds {{< keys "super,n" >}} to its dialog. If you want to remove the remaining configuration, the following works as well:

   ```bash
   sudo apt purge regolith-rofication # remove any remaining config as well
   ```

2. Log out and back in.
3. Test your notifications with `notify-send test123`. You should see a pop-up notification from `dunst`.

D-Bus starts `dunst` when the first notification arrives. To configure it, see `man dunst` and `man 5 dunst`; your own settings go in `~/.config/dunst/dunstrc`.

## Sway: Notification Count in the Bar

In the Wayland session, after Rofication is removed, the bar can show a red `Failed to connect to socket` where the notification count was. Until your `i3status-rs` package hides this block by itself, use your own copy of the bar configuration without it:

1. Copy the default configuration:

   ```bash
   mkdir -p ~/.config/regolith3/i3status-rust
   cp /etc/regolith/i3status-rust/config.toml ~/.config/regolith3/i3status-rust/config.toml
   ```

2. In the copy, delete the `[[block]]` entry with `block = "rofication"` and the `[[block.click]]` lines that follow it.
3. Tell the bar to use your copy, and reload:

   ```bash
   echo "wm.bar.status_config: ~/.config/regolith3/i3status-rust/config.toml" >> ~/.config/regolith3/Xresources
   regolith-look refresh
   ```

## Other Notification Daemons

| Daemon | Package | X11 (i3) | Wayland (sway) |
| --- | --- | --- | --- |
| [dunst](https://dunst-project.org/) | `dunst` | yes | yes |
| [mako](https://github.com/emersion/mako) | `mako-notifier` | no | yes |
| [SwayNotificationCenter](https://github.com/ErikReider/SwayNotificationCenter) | `sway-notification-center` | no | yes |

`mako` and SwayNotificationCenter don't conflict with Rofication, so `apt` leaves it installed, and Rofication keeps receiving the notifications. Remove it yourself:

```bash
sudo apt remove regolith-rofication regolith-wm-rofication-ilia
```

Both only run in the Wayland session. If one of them is your only notification daemon, the X11 session shows no notifications.

## Going Back to Rofication

```bash
sudo apt install regolith-rofication regolith-wm-rofication-ilia
```

This removes `dunst`. Log out and back in.
