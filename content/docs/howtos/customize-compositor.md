---
title: "Customize Compositor"
linkTitle: "How to Customize Compositor"
description: >
  How to customize Regolith's compositor
---

{{< callout type="info" >}}
Pay special attention to this section if you are experiencing odd visual
glitches or slow graphics performance.
{{< /callout >}}

A compositor is a UI component that applies visual effects to windows before
they are rendered on-screen. Many desktop environments integrate a compositor
directly into the window manager, making it difficult to switch out or disable.
In Regolith, the compositor is defined as a pluggable “extension point” in the
packaging system. This means that compositors can be switched out simply by
installing the packages that contain them. The underlying packaging system will
ensure there are no conflicts and that all the dependencies of a given
compositor are also installed.

{{< callout type="info" >}}
This page applies to the X11 session (i3) only. In the Wayland session, sway is
itself the compositor, and the `regolith-compositor-*` packages have no effect.
{{< /callout >}}

{{< callout type="warning" >}}
Since Regolith 3.0, the default compositor of the X11 session is Picom
(`regolith-compositor-picom-glx`). In Regolith 2.x, the default was "no
compositor"; in Regolith 1.x, it was Picom.
{{< /callout >}}

## Finding available compositors

The following command will list all compositors configured to work with
Regolith:

```bash
apt search regolith-compositor-
```

You should find at least the following:

- `regolith-compositor-picom-glx` **[default]**: Uses `picom`. Recommended for most users to
  resolve screen tearing and add visual effects. Modern and well-maintained
  fork of `compton`.
- `regolith-compositor-none`: No compositor. Best performance, no
  visual effects.
- `regolith-compositor-compton-glx`: An ancestor of `picom` which is no longer
  maintained.
- `regolith-compositor-xcompmgr`: An early compositor that may offer more
  consistent performance if more complex compositors are problematic.

## Installing a compositor

{{< callout type="info" >}}
Due to the way that the compositor is managed by i3-wm, **you must log out and
back in for compositor changes to take effect**. Restarting i3 is not
sufficient.
{{< /callout >}}

Run the following package install command to swap out the compositor. In this
case we’ll install `picom`:

```bash
sudo apt install regolith-compositor-picom-glx
```

To run without a compositor, install `regolith-compositor-none` the same way.

**Log out and then log back in** and the new compositor should be loaded. You
can verify by checking the process list for `picom`:

```bash
pgrep -a picom
```

## Overriding Regolith default compositor configuration

### Picom

To provide your own Picom compositor config, copy the default or create
your own and save it as `~/.config/regolith3/picom/config`. It is loaded instead
of the default config `/etc/regolith/picom/config` the next time i3 starts, so
`regolith-look refresh` (which restarts i3) applies it without logging out:

```bash
mkdir -p ~/.config/regolith3/picom
cp /etc/regolith/picom/config ~/.config/regolith3/picom/config
```

Some looks ship their own Picom config and select it with the Xresource
`regolith.compositor.picom.config`. A file in `~/.config/regolith3/picom/config`
takes precedence over it.

If the screen stops updating in the X11 session (new windows do not appear and
workspace switches do not show), for example in a virtual machine without 3D
acceleration, Picom's `glx` backend may not work with software rendering. Set
`backend = "xrender";` in your config, or use `regolith-compositor-none`.

### Compton

`regolith-compositor-compton-glx` reads `~/.config/regolith3/compton/config`
instead of its default config `/etc/regolith/compton/config`.
