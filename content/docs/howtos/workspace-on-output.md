---
title: "Assign a workspace to a specific Output (Display)"
linkTitle: "Assign a workspace to a specific Output (Display)"
description: >
  Specify that a given workspace should always be displayed on a specific monitor
---

i3 and sway can open a workspace on a given output (monitor) every time. To set this up, add `workspace ... output ...` lines to a config partial: a file in your user config directory that Regolith includes after its own configuration. The variables `$ws1` to `$ws19` hold the names of Regolith's workspaces, which you can change as described in [Change Workspace Icons and Labels]({{< ref "change-workspace-icons" >}}).

## X11 Session (i3)

1. Find the name of the output with `xrandr --listmonitors`:

   ```console
   $ xrandr --listmonitors
   Monitors: 2
    0: +*eDP-1 1920/309x1080/173+0+0  eDP-1
    1: +HDMI-1 1920/527x1080/296+1920+0  HDMI-1
   ```

   The output names are in the last column, here `eDP-1` and `HDMI-1`.

2. Create a partial, for example `~/.config/regolith3/i3/config.d/70_workspace-outputs`, with a line for each workspace:

   ```text {filename="~/.config/regolith3/i3/config.d/70_workspace-outputs"}
   workspace "$ws1" output eDP-1
   workspace "$ws2" output HDMI-1
   ```

   Keep the double quotes around the variable: Regolith's workspace names contain spaces.

3. Log out and back in, or reload i3 with {{< keys "super,shift,c" >}}.

{{< callout type="info" >}}
With `regolith-session` older than 1.2.2 (Regolith 3.4 and earlier), i3 reports a `CONFIG` error for these lines right after login ([regolith-desktop#890](https://github.com/regolith-linux/regolith-desktop/issues/890)). Refreshing Regolith with {{< keys "super,shift,r" >}} or `regolith-look refresh` after logging in fixes the workspace names and restarts i3, which then applies the lines.
{{< /callout >}}

## Wayland Session (sway)

1. Find the name of the output with `swaymsg -t get_outputs`:

   ```console
   $ swaymsg -t get_outputs
   Output eDP-1 'AU Optronics 0x573D Unknown' (focused)
   [...]
   Output HDMI-A-1 'ViewSonic Corporation VG2448 V5E192121969'
   [...]
   ```

2. Create a partial, for example `~/.config/regolith3/sway/config.d/70_workspace-outputs`, with a line for each workspace:

   ```text {filename="~/.config/regolith3/sway/config.d/70_workspace-outputs"}
   workspace $ws1 output eDP-1
   workspace $ws2 output HDMI-A-1
   ```

   Write the variable without quotes here. Regolith's sway config already quotes `$ws1` to `$ws19`, and with extra quotes the line doesn't match the workspace.

3. Log out and back in, or reload sway with {{< keys "super,shift,c" >}}.

Because the two sessions need different quoting, use the `i3` and `sway` partial directories for these lines, not `~/.config/regolith3/common-wm/config.d/`.

## More Than One Output

A line can list several outputs. The workspace opens on the first of them that is available. With the following line, workspace 2 opens on the external monitor when it is connected, and on the laptop screen otherwise:

```text {filename="~/.config/regolith3/i3/config.d/70_workspace-outputs"}
workspace "$ws2" output HDMI-1 eDP-1
```

In sway, the same line is `workspace $ws2 output HDMI-A-1 eDP-1`.

See the [i3 user guide](https://i3wm.org/docs/userguide.html#workspace_screen) and `man 5 sway` for details.
