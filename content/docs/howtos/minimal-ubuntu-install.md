---
title: "Minimal Ubuntu Install"
linkTitle: "Minimal Ubuntu Install"
description: >
  Install Regolith on Ubuntu Server
---

This page describes how to install the Regolith Desktop into Ubuntu Server.  This approach produces a minimal setup as many of the standard GNOME desktop packages present in a desktop install will be avoided.

## Steps

### Install Ubuntu Server

Follow online instructions for installing a fresh copy of Ubuntu Server, in a release that Regolith supports. See the [Compatibility Matrix]({{< ref "/docs/getting-started/compatibility" >}}).

### Add Regolith's package key and apt repository

Once Ubuntu Server has been installed, log into a terminal session.  Register the Regolith public key and add the repository URL for your release, as described in the first two steps of the [Install page]({{< ref "/docs/using-regolith/install" >}}).  The same directions are available as text files:

Ubuntu 25.10 (Questing): [install-release-ubuntu-25.10-amd64.txt](/install-release-ubuntu-25.10-amd64.txt)  
Ubuntu 25.04 (Plucky): [install-release-ubuntu-25.04-amd64.txt](/install-release-ubuntu-25.04-amd64.txt)  
Ubuntu 24.04 LTS (Noble): [install-release-ubuntu-24.04-amd64.txt](/install-release-ubuntu-24.04-amd64.txt)  
Ubuntu 22.04 LTS (Jammy): [install-release-ubuntu-22.04-amd64.txt](/install-release-ubuntu-22.04-amd64.txt)  

Example:

```bash
wget https://regolith-desktop.com/install-release-ubuntu-24.04-amd64.txt
cat install-release-ubuntu-24.04-amd64.txt
```

Manually execute each step in the text file, or source the file if you're feeling adventurous.  Once complete, the Regolith Desktop package repositories are installed into the new system.

### Install the Regolith Ubuntu System

There is a special package that is used in the ISO that can also be used in a minimal install such as this: `regolith-system-ubuntu`.  This package includes extra branding not used in typical desktop installs which may share multiple desktop environments.  Run the following command to install Regolith Desktop and all of it's dependencies:

```bash
sudo apt install regolith-system-ubuntu
```

This installs the X11 session (`regolith-session-flashback`) and the `lightdm` login manager.  Some recommended packages also bring in `gdm3` and GNOME Shell, so `apt` asks which display manager to use by default: choose `lightdm`.  If you don't want `gdm3` and GNOME Shell, you can remove them afterwards with `sudo apt remove gdm3 gnome-shell`.

To also install the Wayland session, run:

```bash
sudo apt install regolith-session-sway
```

### Reboot

Once the system boots back up, the `lightdm` login manager will allow you to log into your new Regolith environment.
