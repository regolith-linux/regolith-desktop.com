---
title: "Verify behavior against upstream"
linkTitle: "Verify behavior against upstream"
description: >
  This page describes how to root cause the origin of bugs.
next: /docs/reference
---

Regolith is composed of several existing open-source projects plus some additional custom packages.  Sometimes when a bug is found in Regolith, actually it's a bug in an upstream project.  Determining the source of bugs is a first step in getting them fixed.

## Check `gnome-flashback`

Most of the GNOME support in Regolith's X11 session (`regolith-session-flashback`) comes from the `gnome-flashback` project.  To run a `gnome-flashback` session, install the session and log back in to your account after selecting that session from the login screen.

1. Install the `gnome-session-flashback` session:

   ```bash
   sudo apt install gnome-session-flashback
   ```

   `apt` also removes `regolith-session-flashback-ext`, which conflicts with `gnome-session-flashback`. On Ubuntu 26.04 the Regolith X11 session keeps working without it.

2. Log out. On the login screen, select your user, choose the `GNOME Flashback (Metacity)` session (in GDM, from the gear button in the lower-right corner), and log in.

3. Verify your behavior against Regolith.  If the problematic behavior also exists in `gnome-flashback` then the bug is likely there.  If the bug does not exist in the `gnome-flashback` then it's likely to be in Regolith.

4. When you are done, log back into the `Regolith / X11` session. To remove the GNOME Flashback session again, reinstall the package it replaced:

   ```bash
   sudo apt install regolith-session-flashback-ext
   ```

## Wayland session

The Wayland session (`regolith-session-sway`) does not use `gnome-flashback`. It runs Regolith's build of Sway (`sway-regolith`, which replaces Ubuntu's `sway` package) together with GNOME services such as `gnome-settings-daemon`. To check whether a problem comes from a GNOME application or service, compare with the GNOME session of your distribution, if it is installed. Problems with window management are more likely to be in Sway or in Regolith's configuration.
