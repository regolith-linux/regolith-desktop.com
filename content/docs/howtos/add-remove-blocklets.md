---
title: 'Change Bar Status indicators'
linkTitle: 'Change Bar Status indicators'
description: >
    Make changes to the status indicators in the bar.
---

The X11 session (i3) and the Wayland session (sway) use different status bar programs:

- **X11:** [`i3xrocks`]({{< ref "/docs/reference/i3xrocks.md" >}}). Each status indicator is a separate `i3xrocks-*` package.
- **Wayland:** [`i3status-rs`](https://github.com/greshake/i3status-rust), configured by a single file. The `i3xrocks-*` packages have no effect there; see [Wayland (sway)](#wayland) below.

## Adding or Removing status indicators

### Finding Status Indicators

```bash
apt search i3xrocks-
```

### Installing a Status Indicator

```bash
sudo apt install i3xrocks-memory
regolith-look refresh
```

To remove an indicator, remove its package (for example `sudo apt remove i3xrocks-memory`) and run `regolith-look refresh`.

## Customization of the i3 bar {#customize-bar}

Each status indicator on the bar is managed by a file. The name of the file maps to the position of the status indicator on the bar, e.g. a file's name starting with `10_` mean its corresponding indicator will be positioned before an indicator which has a configuration file starting with `20_`. Within each file there is some information that `i3xrocks` uses to execute a script that ultimately returns the data you see on the bar. Some of the status indicators have configuration parameters that can be adjusted to your liking. For example, if you would prefer that the battery status changes more frequently, the polling interval can be updated.

{{< callout type="info" >}}
This section describes Regolith 3.6 and later. Up to Regolith 3.5, the bar reads either your files or the package files, never both: see [Regolith 3.5 and earlier](#customize-bar-3-5).
{{< /callout >}}

`i3xrocks` reads two directories together: the package files in `/usr/share/i3xrocks/conf.d/` and your own files in `~/.config/regolith3/i3xrocks/conf.d`. It sorts the files of both directories by name, so your files take their place among the package ones. You only need files for what you want to change:

-   **Add an indicator:** create a file with a new name. For example, `50_hello` appears between `40_cpu-usage` and `90_time`.
-   **Replace an indicator:** create a file with the same name as the package file. Your file is read instead of the package file.
-   **Hide an indicator:** create an empty file with the same name as the package file.
-   **Change some settings of an indicator:** write a section with the same name (and the same `instance`, if it has one) as the package block. Your section sets only the properties you write in it, and the indicator keeps the rest of its package settings. The indicator is shown at the position of your file, so copying a file under a different number moves the indicator rather than showing it twice.

The global properties in `01_setup` (the command, the colors) apply to your files too. Indicator packages you install later are added to the bar without any change to your files.

For example, to show the CPU indicator before the network traffic indicator, not display the memory indicator, and add a label to the clock, perform the following steps (the files in `/usr/share/i3xrocks/conf.d/` depend on the indicator packages you have installed):

```console
$ ls /usr/share/i3xrocks/conf.d/
01_setup
30_net-traffic
40_cpu-usage
50_memory
90_time
$ mkdir -p ~/.config/regolith3/i3xrocks/conf.d
$ cd ~/.config/regolith3/i3xrocks/conf.d
$ cp /usr/share/i3xrocks/conf.d/40_cpu-usage 20_cpu-usage
$ touch 50_memory
$ printf '[time]\nlabel=TIME \n' > 95_time-label
$ pkill -HUP i3xrocks
```

In the above steps the following actions are performed:

-   The current `i3xrocks` modules are listed that exist in the directory created by regolith
-   The directory where user configuration lives is created at `~/.config/regolith3/i3xrocks/conf.d`
-   `40_cpu-usage` is copied as `20_cpu-usage`, which moves the CPU indicator in front of the network traffic indicator
-   The empty `50_memory` hides the memory indicator
-   `95_time-label` adds a label to the clock; the clock keeps its command and interval
-   `pkill -HUP i3xrocks` makes the bar read its configuration again, without restarting i3. `regolith-look refresh` works as well

To go back to the defaults, remove your files, or the whole directory, and run `pkill -HUP i3xrocks` again.

**Note**: If you staged the bar in Regolith 3.5 or earlier by copying every indicator you wanted to keep, your bar stays the same, because your files replace the package files with the same names. The indicators you didn't copy come back, though. To hide one again, create an empty file with its name, for example `touch ~/.config/regolith3/i3xrocks/conf.d/50_memory`.

### Regolith 3.5 and earlier {#customize-bar-3-5}

The first thing to do in order to customize the bar is to copy the indicators you wish to see from `/usr/share/i3xrocks/conf.d/` to `~/.config/regolith3/i3xrocks/conf.d`. Each file in `~/.config/regolith3/i3xrocks/conf.d` can be modified as you see fit. To change the order of status indicators on the bar, simply change the name of the files to the sort order you prefer. Once you've made your changes, refreshing the session should cause the bar to update based on your new configuration.

The same example as above, without the clock label:

```console
$ mkdir -p ~/.config/regolith3/i3xrocks/conf.d
$ cp /usr/share/i3xrocks/conf.d/01_setup ~/.config/regolith3/i3xrocks/conf.d/01_setup
$ cp /usr/share/i3xrocks/conf.d/40_cpu-usage ~/.config/regolith3/i3xrocks/conf.d/20_cpu-usage
$ cp /usr/share/i3xrocks/conf.d/30_net-traffic ~/.config/regolith3/i3xrocks/conf.d/30_net-traffic
$ cp /usr/share/i3xrocks/conf.d/90_time ~/.config/regolith3/i3xrocks/conf.d/90_time
$ regolith-look refresh
```

`01_setup` holds the settings shared by all indicators, so keep it. Renaming `40_cpu-usage` to `20_cpu-usage` moves the CPU indicator in front of the network traffic indicator, and the memory indicator is not copied, so it is not shown.

**Note**: If the directory `~/.config/regolith3/i3xrocks/conf.d` exists, the defaults in `/usr/share/i3xrocks/conf.d/` are ignored, even if the directory is empty. Copy every indicator you want to keep, and remove the directory to go back to the defaults. Indicator packages you install later are not added to the bar until you copy their file as well. Sending `SIGHUP` to `i3xrocks` stops the bar instead of reloading it, so use `regolith-look refresh`.

## Wayland (sway) {#wayland}

The sway bar runs `i3status-rs` with `/etc/regolith/i3status-rust/config.toml`. To change the indicators, copy that file, edit the copy, and point the Xresource `wm.bar.status_config` at it:

```bash
mkdir -p ~/.config/regolith3/i3status-rust
cp /etc/regolith/i3status-rust/config.toml ~/.config/regolith3/i3status-rust/config.toml
echo "wm.bar.status_config: ~/.config/regolith3/i3status-rust/config.toml" >> ~/.config/regolith3/Xresources
```

Each `[[block]]` section in the file is one indicator, shown in the order of the file. Remove a section to hide an indicator, move it to change the order, or add a block, for example the CPU indicator:

```toml {filename="~/.config/regolith3/i3status-rust/config.toml"}
[[block]]
block = "cpu"
```

Then run `regolith-look refresh`. The available blocks and their options are described in the [i3status-rust documentation](https://docs.rs/i3status-rs/latest/i3status_rs/blocks/index.html).

## Further Reading

The [reference page for configurations]({{< ref "/docs/Reference/configurations.md" >}}) has more details about the configuration files used with Regolith Linux.
