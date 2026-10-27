---
title: "Launch Default i3 Session"
linkTitle: "Launch Default i3 Session"
description: >
  Run plain i3 without uninstalling Regolith
---

Regolith 3 runs i3 from the `i3-wm` package, and that package also installs a plain i3 session. No extra package is needed to use i3 without Regolith: on the login screen, select your user, click the gear button in the lower right corner, and choose **i3** instead of **Regolith / X11**.

The plain session uses i3's own configuration, `~/.config/i3/config`, and none of Regolith's configuration, bar or looks. When there is no such file, `i3-config-wizard` offers to create one at the first start.

i3 has had gaps built in since version 4.22, and Regolith 3 ships i3 4.22 or later. The `gaps` and `smart_gaps` settings therefore work in the plain session too, and the `i3-gaps` and `i3-gaps-session` packages of earlier Regolith releases no longer exist.

There is no plain sway session. Regolith's `sway-regolith` package replaces the distribution's `sway` package and only installs the **Regolith / Wayland** session.

## Use Case

This is useful for, say, running an application that does not integrate well with GNOME, or for checking whether a problem also happens in i3 without Regolith. To compare against GNOME instead, see [Verify behavior against upstream]({{< ref "gnome-flashback-session" >}}).
