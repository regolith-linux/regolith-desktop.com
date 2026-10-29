---
title: "Launch an app in a Floating Window"
linkTitle: "Launch an app in a Floating Window"
description: >
  Most GUI apps can be launched in floating window mode.
---

Most X11 applications support a flag `--class` to specify the `class` under which the application runs.  Regolith is configured such that apps with a class of `floating_window` will launch with i3's floating window mode.

## Examples

### Firefox

Launch Firefox in floating mode:

```bash
firefox --class=floating_window
```

### Alacritty

Launch the Alacritty terminal in floating mode:

```bash
alacritty --class floating_window
```

### `gnome-terminal`

GNOME Terminal ignores `--class`: its windows are created by a shared `gnome-terminal-server` process, so they keep the class `Gnome-terminal`. To float all GNOME Terminal windows instead, add a file to `~/.config/regolith3/i3/config.d/` (see [Customize i3 Configuration]({{< ref "customize-i3-configuration" >}})) with the line `for_window [class="Gnome-terminal"] floating enable`, and reload i3 with {{< keys "super,shift,c" >}}.

## Sway

In the Wayland session the `floating_window` rule matches only X11 applications that run through XWayland. Native Wayland applications have an `app_id` instead of a class. To float them, add a rule for that `app_id` in `~/.config/regolith3/sway/config.d/`:

```text {filename="~/.config/regolith3/sway/config.d/80_floating_window"}
for_window [app_id="floating_window"] floating enable
```

Then reload Sway with {{< keys "super,shift,c" >}} and set the `app_id` when launching the application:

```bash
alacritty --class floating_window
firefox --name floating_window
```

`swaymsg -t get_tree` shows the `app_id` of open windows.
