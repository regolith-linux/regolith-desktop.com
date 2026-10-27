---
title: "Change the Bar Date and Time Format"
linkTitle: "Change the Bar Date and Time Format"
description: >
  Change the date/time format used in the bar
---

The X11 (i3) and Wayland (sway) sessions use different bars, which are configured differently.

## X11 Session (i3)

The time format (12 or 24 hour) can be specified in Settings, under **System > Date & Time > Time Format**. The bar picks up the change within about ten seconds.

{{< img src="settings-time-format.png" alt="The Date & Time page of Settings, with the Time Format switch set to 24-hour" >}}

The order of day and month follows the country of your `LC_TIME` locale: month first (`MM/DD`) for the United States, Canada, China, Japan and a few other countries, and day first (`DD/MM`) for all others.

The date and time formats can also be explicitly set via `Xresources`. To do so, override the `i3xrocks.date.format2` `Xresource` key in `~/.config/regolith3/Xresources`. The value is the format that would be passed to the `date` command on the command line, without the leading `+`. For example, the following line uses the `YYYY-MM-DD hh:mm` format:

```yaml {filename="~/.config/regolith3/Xresources"}
i3xrocks.date.format2: %Y-%m-%d %H:%M
```

Then run `regolith-look refresh`. The bar shows the date and time like this:

{{< img src="bar-x11.png" alt="The date and time on the X11 bar: 2026-10-27 22:27" >}}

While this key is set, the bar no longer follows the Time Format setting.

The most useful formats that are supported are as follows:

-   `%Y` prints year as `YYYY`
-   `%y` prints year as `YY`
-   `%m` prints month as `MM`
-   `%d` prints day as `DD`
-   `%H` prints hours in 24 hour format
-   `%I` prints hours in 12 hour format
-   `%M` prints minute as `mm`
-   `%p` prints `AM` or `PM`

For more details and placeholders, please look at `date --help`.

## Wayland Session (sway)

The sway bar, `i3status-rs`, doesn't read `i3xrocks.date.format2` or the Time Format setting. Its date and time formats are part of its configuration file. To change them, use your own copy of that file:

1. Copy the default configuration:

   ```bash
   mkdir -p ~/.config/regolith3/i3status-rust
   cp /etc/regolith/i3status-rust/config.toml ~/.config/regolith3/i3status-rust/config.toml
   ```

2. In the copy, change the `format` of the two `time` blocks. By default, the date block uses `%a-%d/%m/%y` and the time block uses `%R` (24 hour `hh:mm`). For example, for `YYYY-MM-DD` and a 12 hour clock:

   ```toml {filename="~/.config/regolith3/i3status-rust/config.toml"}
   [[block]]
   block = "time"
   interval = 5
   format = "\uf073 $timestamp.datetime(f:'%Y-%m-%d')"

   [[block]]
   block = "time"
   interval = 5
   format = "$icon $timestamp.datetime(f:'%I:%M %p')"
   ```

   On Ubuntu 22.04 and Debian, the file has a single `time` block with a plain format, `format = "%d/%m %R"`; change that one instead.

3. Tell the bar to use your copy, and reload:

   ```bash
   echo "wm.bar.status_config: ~/.config/regolith3/i3status-rust/config.toml" >> ~/.config/regolith3/Xresources
   regolith-look refresh
   ```

   With the example above, the bar shows the date and time like this:

   {{< img src="bar-sway.png" alt="The date and time on the Wayland bar: 2026-10-27 and 10:24 PM" >}}

The placeholders are the same as for `date`, listed above. See the [`time` block documentation](https://docs.rs/i3status-rs/latest/i3status_rs/blocks/time/index.html) for the other options.
