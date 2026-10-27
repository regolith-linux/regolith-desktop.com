---
title: "Support Multiple Batteries"
linkTitle: "Support Multiple Batteries"
description: >
  Display status of the batteries per instance
---

Out of the box the bar shows the combined charge of all batteries: in the X11 session once the `i3xrocks-battery` package is installed, and in the Wayland session by default. For some laptops that have multiple batteries, e.g. ThinkPads, you may want to have each battery displayed.

## i3

1. Install the battery blocklet if it is not installed yet:

```bash
sudo apt install i3xrocks-battery
```

2. [Stage your i3xrocks configuration files]({{< ref "add-remove-blocklets#customize-bar" >}}), including `80_battery`, then open `~/.config/regolith3/i3xrocks/conf.d/80_battery` in your editor of choice. Once `~/.config/regolith3/i3xrocks/conf.d` contains files, the bar shows only the blocklets in that directory, so copy every blocklet you want to keep.
3. Find the line that pertains to the battery blocklet:

```toml {filename="~/.config/regolith3/i3xrocks/conf.d/80_battery"}
# Battery indicator
#
# Displays total percentage charge left of specified battery.
# You can specify the instance by adding:
# instance=BAT0
[battery]
interval=10
```

4. List all the batteries on your machine by running:

```sh
upower --enumerate

# /org/freedesktop/UPower/devices/line_power_AC
# /org/freedesktop/UPower/devices/battery_BAT0
# /org/freedesktop/UPower/devices/battery_BAT1
# /org/freedesktop/UPower/devices/DisplayDevice
```

5. Update the staged config file to have one blocklet per battery, using the UPower device names (`battery_BAT0`, `battery_BAT1`) as the instance:

```toml {filename="~/.config/regolith3/i3xrocks/conf.d/80_battery"}
...
# Battery indicator
#
# Displays total percentage charge left of specified battery.
[battery]
interval=10
instance=battery_BAT0

[battery]
interval=10
instance=battery_BAT1
```

6. Refresh the bar:

```bash
regolith-look refresh
```

## Sway

1. Stage your i3status-rust config file:
```bash
mkdir -p ~/.config/regolith3/i3status-rust
cp /etc/regolith/i3status-rust/config.toml ~/.config/regolith3/i3status-rust/config.toml
```

2. Configure to supply i3status-rust with your config file:
```bash
echo "wm.bar.status_config: ~/.config/regolith3/i3status-rust/config.toml" >> ~/.config/regolith3/Xresources
```

3. List all the battery instances running on your machine by running:

```bash
ls /sys/class/power_supply/

# AC BAT0 BAT1
```

4. In the staged config file, replace the `battery` block (which shows the combined charge, `device = "DisplayDevice"`) with one block per battery:

```toml {filename="~/.config/regolith3/i3status-rust/config.toml"}
[[block]]
block = "battery"
interval = 10
device = "BAT0"

[[block]]
block = "battery"
interval = 10
device = "BAT1"
```

5. Refresh the session, or log back in:

```bash
regolith-look refresh
```

See the [i3status-rust battery block documentation](https://docs.rs/i3status-rs/latest/i3status_rs/blocks/battery/index.html) for further options.

## Further Reading

See the [reference page for configurations]({{< ref "/docs/Reference/configurations.md" >}}) for more details about config files in Regolith.
