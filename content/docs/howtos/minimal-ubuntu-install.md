---
title: "Minimal Install"
linkTitle: "Minimal Install"
description: >
  Install Regolith Desktop on Debian/Ubuntu Server
---

This page describes how to install the Regolith Desktop on Debian/Ubuntu Server. This approach produces a minimal setup as many of the standard GNOME desktop packages present in a desktop install will be avoided.

## Steps

### Install Server Editions

Debian
* Download: https://www.debian.org/distrib/netinst
* Instructions: https://www.debian.org/releases/bookworm/amd64/index.en.html

Ubuntu
* Download: https://ubuntu.com/download/server
* Instructions: https://documentation.ubuntu.com/server/tutorial/basic-installation


### Download Regolith installer

Once the server edition has been installed, log into a terminal session. Use the correspinding URL for your distribution and version to download the installation directions as a text file.

| Distro | Version   | Codename | URL                                                                                                                                        |
| ------ | --------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------ |
| Debian | 12        | Bookworm | [https://regolith-desktop.com/install-release-debian-12-amd64.txt](https://regolith-desktop.com/install-release-debian-12-amd64.txt)       |
| Debian | 11        | Bullseye | [https://regolith-desktop.com/install-release-debian-11-amd64.txt](https://regolith-desktop.com/install-release-debian-11-amd64.txt)       |
| Ubuntu | 25.04     | Plucky   | [https://regolith-desktop.com/install-release-ubuntu-25.04-amd64.txt](https://regolith-desktop.com/install-release-ubuntu-25.04-amd64.txt) |
| Ubuntu | 24.10     | Oracular | [https://regolith-desktop.com/install-release-ubuntu-24.10-amd64.txt](https://regolith-desktop.com/install-release-ubuntu-24.10-amd64.txt) |
| Ubuntu | 24.04 LTS | Noble    | [https://regolith-desktop.com/install-release-ubuntu-24.04-amd64.txt](https://regolith-desktop.com/install-release-ubuntu-24.04-amd64.txt) |
| Ubuntu | 23.10     | Mantic   | [https://regolith-desktop.com/install-release-ubuntu-23.10-amd64.txt](https://regolith-desktop.com/install-release-ubuntu-23.10-amd64.txt) |
| Ubuntu | 23.04     | Lunar    | [https://regolith-desktop.com/install-release-ubuntu-23.04-amd64.txt](https://regolith-desktop.com/install-release-ubuntu-23.04-amd64.txt) |
| Ubuntu | 22.10     | Kinetic  | [https://regolith-desktop.com/install-release-ubuntu-22.10-amd64.txt](https://regolith-desktop.com/install-release-ubuntu-22.10-amd64.txt) |
| Ubuntu | 22.04 LTS | Jammy    | [https://regolith-desktop.com/install-release-ubuntu-22.04-amd64.txt](https://regolith-desktop.com/install-release-ubuntu-22.04-amd64.txt) |
| Ubuntu | 20.04 LTS | Focal    | [https://regolith-desktop.com/install-release-ubuntu-20.04-amd64.txt](https://regolith-desktop.com/install-release-ubuntu-20.04-amd64.txt) |

Examples
```bash
# General Syntax
wget URL
cat install-release-distro-version-amd64.txt

# For Debian 12
wget https://regolith-desktop.com/install-release-debian-12-amd64.txt
cat install-release-debian-12-amd64.txt

# For Ubuntu 24.04
wget -O https://regolith-desktop.com/install-release-ubuntu-24.04-amd64.txt
cat install-release-ubuntu-24.04-amd64.txt
```

## Install Regolith packages

Manually execute each step in the installer text file. Once complete, the Regolith Desktop package repositories should be installed into the new system.

### Reboot

```
$ systemctl reboot
```

Once the system boots back up, the `lightdm` login manager will allow you to log into your new Regolith environment.
