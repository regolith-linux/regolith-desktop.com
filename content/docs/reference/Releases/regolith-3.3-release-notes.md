---
title: "3.3 Release Notes"
linkTitle: "3.3 Release Notes"
weight: 84
description: >
  Release notes for Regolith 3.3
prev: /docs/reference/releases
---

## Regolith 3.3 Release Notes

Regolith 3.3 is a minor release focusing on Ubuntu 25.04 support, numerous build system improvements, and sway 1.10.

## Features

* Ubuntu 25.04 (Plucky) support

## Known Issues

* `regolith-control-center` has *not* been ported to GNOME 47+ and so users on Ubuntu 24.10 or newer need to use `gnome-control-center` instead.

## Installation Instructions

TBD

Installation on a freshly installed Ubuntu or Debian system can follow the standard installation instructions.  When upgrading from a previous release
using `do-release-upgrade`, the Regolith package repo has to be re-added to your system as the installer removes it.  Ensure the following packages are installed after the Ubuntu release process has completed and the Regolith package repository has been added back to your `apt` configuration: 

* `regolith-i3-ilia` and/or `regolith-sway-ilia`
* `regolith-i3-control-center-gnome` and/or `regolith-sway-control-center-gnome` on 24.10 or 25.04 Ubuntu versions.

After `apt upgrade` is complete, verify that Regolith 3.3 is installed by:

```console
$ cat /etc/regolith/version 
REGOLITH_VERSION=3.3
```

## Changes since Regolith 3.2

TBD
