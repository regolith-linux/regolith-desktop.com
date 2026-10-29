---
title: "Find and Install Packages"
linkTitle: "Find and Install Packages"
description: >
  Search for packages and install them on your system.
---

Regolith is a Debian-based system. Debian packaging tools can be used to find and install packages.

## Find package via the command line

The `apt` command can be used to query installed or all packages:

```bash
apt list --installed <package name> # finds installed packages
apt list <package name> # finds all packages
```

For example, to list all official Regolith looks that could be installed, run:

```console
$ apt list 'regolith-look-*'
...
```

Quote patterns such as `regolith-look-*` so that the shell does not expand them to file names first.

To see which status indicators are installed on your system, run:

```bash
apt list --installed 'i3xrocks-*'
```

The `i3xrocks-*` status indicators are used by the bar of the X11 session. The bar of the Wayland session uses `i3status-rs`, whose blocks are configured in a file rather than installed as packages. See [Install the Sway Compositor]({{< ref "install-sway#bar-changes" >}}).

## Install packages via the command line

To install packages via the command line, again we can us the `apt` command, but must use sudo to gain the rights necessary for installation:

```bash
sudo apt install <package name>
```

To install the battery status indicator, run:

```bash
sudo apt install i3xrocks-battery
```

New status indicators appear on the bar after `regolith-look refresh` or the next login, unless you have [staged your own i3xrocks configuration]({{< ref "add-remove-blocklets#customize-bar" >}}). In that case, copy the indicator's file from `/usr/share/i3xrocks/conf.d/` as well.

## Find and Install Packages via GUI app

There are a number of apps that can provide a nice window interface on top of the apt packaging system, for example `synaptic`. Refer to the documentation for the tool of your choice to learn how to search for and install packages.
