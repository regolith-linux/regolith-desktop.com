---
title: "3.4 Release Notes"
linkTitle: "3.4 Release Notes"
weight: 83
description: >
  Release notes for Regolith 3.4
prev: /docs/reference/releases
---

## Regolith 3.4 Release Notes

Regolith 3.4 is a minor release focusing on Ubuntu 25.10 and Debian 13 support.

## Features

* Ubuntu 25.10 (Questing) support
* Debian 13 (Trixie) support

## Known Issues

No known issues at the time of the release.

## Installation Instructions

Refer to the [installation instructions](/docs/using-regolith/install/) to install or upgrade from a previous version.

After the Regolith upgrade is complete, verify that Regolith 3.4 is installed by:

```console
$ cat /etc/regolith/version 
REGOLITH_VERSION=3.4
```

## Changes since Regolith 3.3

### Changes in [`arc-icon-theme`](https://github.com/regolith-linux/arc-icon-theme/releases/v20251012-1)

```text
392d7cc feat: use build-only to test pull request
```

### Changes in [`armesto`](https://github.com/regolith-linux/armesto/releases/v0.2.3ubuntu1)

```text
3bbb44e fix: use correct version spec to avoid newer, rustc-incompat dependencies
348a3d0 feat: use build-only to test pull request
```

### Changes in [`ayu-theme`](https://github.com/regolith-linux/ayu-theme/releases/v0.2.4-1)

```text
e23b62d feat: use build-only to test pull request
```

### Changes in [`childe`](https://github.com/regolith-linux/childe/releases/v0.1.4)

```text
b7f4742 feat: use build-only to test pull request
```

### Changes in [`dracula-gtk`](https://github.com/regolith-linux/dracula-gtk/releases/v1.1.3)

```text
f8c03d4 feat: use build-only to test pull request
```

### Changes in [`fonts-jetbrains-mono`](https://github.com/regolith-linux/fonts-jetbrains-mono/releases/v1.0.3-1regolith1)

```text
e835009 feat: use build-only to test pull request
```

### Changes in [`fonts-materialdesignicons-webfont`](https://github.com/regolith-linux/fonts-materialdesignicons-webfont/releases/v1.6.50-5regolith1)

```text
3dbf43c feat: use build-only to test pull request
```

### Changes in [`fonts-source-code-pro-ttf`](https://github.com/regolith-linux/fonts-source-code-pro-ttf/releases/v1.011-0ubuntu1-ppa3)

```text
5db0bb1 feat: use build-only to test pull request
```

### Changes in [`gruvbox-gtk`](https://github.com/regolith-linux/gruvbox-gtk/releases/v1.0.2-2)

```text
edb2f30 feat: use build-only to test pull request
```

### Changes in [`gtklock`](https://github.com/regolith-linux/gtklock/releases/v2.2.2)

```text
ef9761f feat: use build-only to test pull request
```

### Changes in [`i3-next-workspace`](https://github.com/regolith-linux/i3-next-workspace/releases/v1.0.6)

```text
f96fbb1 feat: use build-only to test pull request
```

### Changes in [`i3-snapshot`](https://github.com/regolith-linux/i3-snapshot/releases/v1.1-2ubuntu1-ppa1)

```text
d33744c fix: properly checkout submodules
123efe9 feat: use build-only to test pull request
```

### Changes in [`i3-swap-focus`](https://github.com/regolith-linux/i3-swap-focus/releases/v0.4.5)

```text
6d35c33 feat: use build-only to test pull request
```

### Changes in [`i3status-rs`](https://github.com/regolith-linux/i3status-rs_debian/releases/v0.32.1-2)

```text
304cd81 feat: use build-only to test pull request
```

### Changes in [`i3xrocks`](https://github.com/regolith-linux/i3xrocks/releases/v1.3.6-2)

```text
7f5692e feat: use build-only to test pull request
```

### Changes in [`ilia`](https://github.com/regolith-linux/ilia/releases/v0.18.0)

```text
4822ffa Clean up leftover prints from debugging
6587204 Force remove sp_after_sparen
41d1cef Run uncrustify again
c95be02 Default to using release build types
d04df63 Uncrustify everything
5d1a893 Fix screenshot link in readme
9230027 Added debug instructions and timers
ecbdf9d Load icons async
7385909 Fix prefix matching to sort correct
20d2030 Extract app sorting into a dedicated function for testing
687835a Respect icon size=0 harder
d5782f5 Removed newlines in debug() statements
3f89b1b gobject-introspection is not necessary when using wrapGAppsHook3
6528a93 flake with packaging for ilia
```

### Changes in [`lago`](https://github.com/regolith-linux/lago/releases/v0.2.0-2)

```text
8a1cb1b feat: use build-only to test pull request
```

### Changes in [`libtrawldb`](https://github.com/regolith-linux/libtrawldb/releases/v0.1-4)

```text
16d1884 feat: use build-only to test pull request
```

### Changes in [`nordic`](https://github.com/regolith-linux/nordic/releases/v2.1.0-3)

```text
b401873 feat: use build-only to test pull request
```

### Changes in [`picom`](https://github.com/regolith-linux/picom/releases/v11.2.4)

```text
4f54e28 feat: use build-only to test pull request
```

### Changes in [`python3-i3ipc`](https://github.com/regolith-linux/python3-i3ipc/releases/v2.1.1-1ubuntu1-ppa9)

```text
cb25e0f feat: use build-only to test pull request
```

### Changes in [`regolith-avizo`](https://github.com/regolith-linux/avizo/releases/v0.1.5)

```text
74db442 feat: use build-only to test pull request
```

### Changes in [`regolith-compositor-compton-glx`](https://github.com/regolith-linux/regolith-compositor-compton-glx/releases/v1.2.2)

```text
3023a6a feat: use build-only to test pull request
```

### Changes in [`regolith-compositor-none`](https://github.com/regolith-linux/regolith-compositor-none/releases/v1.0.4-2)

```text
2738d16 feat: use build-only to test pull request
```

### Changes in [`regolith-compositor-picom-glx`](https://github.com/regolith-linux/regolith-compositor-picom-glx/releases/v1.4.0-1)

```text
f83a5ea feat: use build-only to test pull request
```

### Changes in [`regolith-compositor-xcompmgr`](https://github.com/regolith-linux/regolith-compositor-xcompmgr/releases/v1.4.0-2)

```text
b152af8 feat: use build-only to test pull request
```

### Changes in [`regolith-control-center`](https://github.com/regolith-linux/regolith-control-center/releases/v1.46.0-5-gnome-46)

```text
a24afe776 feat: use build-only to test pull request
```

### Changes in [`regolith-default-settings`](https://github.com/regolith-linux/regolith-default-settings/releases/v2.0.6)

```text
b591be3 feat: use build-only to test pull request
```

### Changes in [`regolith-desktop`](https://github.com/regolith-linux/regolith-desktop/releases/v4.10.0)

```text
0c0ca2d feat: use build-only to test pull request
```

### Changes in [`regolith-displayd`](https://github.com/regolith-linux/regolith-displayd/releases/v0.3.3)

```text
6d85c49 feat: use build-only to test pull request
```

### Changes in [`regolith-distro-ubuntu`](https://github.com/regolith-linux/regolith-distro-ubuntu/releases/v2.0.0-3)

```text
6c891a9 feat: use build-only to test pull request
```

### Changes in [`regolith-ftue`](https://github.com/regolith-linux/regolith-ftue/releases/v2.2.2)

```text
4a8afbf feat: use build-only to test pull request
```

### Changes in [`regolith-i3xrocks-config`](https://github.com/regolith-linux/regolith-i3xrocks-config/releases/v5.5.3)

```text
98fba35 volume: avoid logging error while detecting pulseaudio (#157)
25628ec feat: use build-only to test pull request
```

### Changes in [`regolith-inputd`](https://github.com/regolith-linux/regolith-inputd/releases/v0.4.1)

```text
9f6b665 feat: use build-only to test pull request
```

### Changes in [`regolith-lightdm-config`](https://github.com/regolith-linux/regolith-lightdm-config/releases/v1.3.8)

```text
c2011f2 feat: use build-only to test pull request
```

### Changes in [`regolith-look-default`](https://github.com/regolith-linux/regolith-look-default/releases/v0.8.4)

```text
0d18541 feat: use build-only to test pull request
```

### Changes in [`regolith-look-extra`](https://github.com/regolith-linux/regolith-look-extra/releases/v0.9.3)

```text
3b06fe5 feat: use build-only to test pull request
```

### Changes in [`regolith-powerd`](https://github.com/regolith-linux/regolith-powerd/releases/v0.7.0)

```text
6787a7c fix: use the correct action command for locking
1bcbf00 fix: swayidle doesn't resume if timeout action in-flight
6067fe6 chore!: update the Xresource key for lock program
753d162 fix: prevent locking upto 5sec after display off
79ca736 feat: autolock 5 seconds after idle timeout
9c670f0 feat: use build-only to test pull request
```

### Changes in [`regolith-rofi-config`](https://github.com/regolith-linux/regolith-rofi-config/releases/v1.4.2-2)

```text
531fcb1 feat: use build-only to test pull request
```

### Changes in [`regolith-rofication`](https://github.com/regolith-linux/regolith-rofication/releases/v1.5.3)

```text
e0a4d20 fix: add new dependency in ubuntu-plucky and similar.  Fixes build issue w/ distutils
997f79b Use xrescat in PATH (#21)
ca36bc6 feat: use build-only to test pull request
```

### Changes in [`regolith-session`](https://github.com/regolith-linux/regolith-session/releases/v1.2.0-1ubuntu1)

```text
df1a9e6 refactor: cause x11 session to be launched by systemd. (#60)
1696d3e feat: sway-audio-idle-inhibit as recommends dependency
f7ccf19 feat: use build-only to test pull request
```

### Changes in [`regolith-sway-touchpad-gestures`](https://github.com/regolith-linux/regolith-sway-touchpad-gestures/releases/v0.1.0-4)

```text
e55f029 feat: use build-only to test pull request
```

### Changes in [`regolith-system-ubuntu`](https://github.com/regolith-linux/regolith-system-ubuntu/releases/v1.0.1-1)

```text
9339206 feat: use build-only to test pull request
```

### Changes in [`regolith-unclutter-xfixes`](https://github.com/regolith-linux/regolith-unclutter-xfixes/releases/v1.5-4)

```text
4f24f59 feat: use build-only to test pull request
```

### Changes in [`regolith-wm-config`](https://github.com/regolith-linux/regolith-wm-config/releases/v4.11.11)

```text
fe8b80d chore!: use seperate Xresource keys for lock program
45f8a76 fix: set im-config preference to none
8c51ce1 Add stacking layout to default layout toggle list
```

### Changes in [`solarc-theme`](https://github.com/regolith-linux/solarc-theme/releases/v800c997-5)

```text
e989fe6 feat: use build-only to test pull request
```

### Changes in [`sway-audio-idle-inhibit`](https://github.com/regolith-linux/SwayAudioIdleInhibit/releases/v2.0.0)

```text
110d61a feat: add systemd to handle process execution and failures
76cb8d4 Bumped version to 0.2.0
234f1ab Updated build scripts
e903b34 Use systemd/elogind Inhibit instead of the wayland protocol
3b092d3 Added .clang-format file
57bcbe9 Move registry_listener from stack into static memory
c754fa8 Added gcc as build dependency
71dc6df Added COPR RPM spec file
58d57e7 feat: use build-only to test pull request
e660559 Bumped version to 0.1.2
47e8150 Safe handling of ignoredSourceOutputs array
```

### Changes in [`sway-regolith`](https://github.com/regolith-linux/sway-regolith/releases/v1.9-18)

```text
c108121e feat: use build-only to test pull request
```

### Changes in [`trawl`](https://github.com/regolith-linux/trawl/releases/v0.2.5-1)

```text
a4b1915 Revert "Merge pull request #8 from manthanabc/master"
20b6497 Revert "chore: update cargo.lock"
39d333e Revert "chore: bump versions"
1967d03 feat: use build-only to test pull request
```

### Changes in [`whitesur-gtk-theme`](https://github.com/regolith-linux/WhiteSur-gtk-theme/releases/v1.1-2025.10.13)

```text
68412a4 feat: use build-only to test pull request
```

### Changes in [`xdg-desktop-portal-regolith`](https://github.com/regolith-linux/xdg-desktop-portal-regolith/releases/v0.3.11-1)

```text
9636a39 fix: disable newly added gnome xdp interfaces
d697c9c feat: use none instead of regolith backend for disabling interfaces
50aebb5 fix: dark mode not working (#18)
```

### Changes in [`xrescat`](https://github.com/regolith-linux/xrescat/releases/v1.2.1-4)

```text
bfe72ef feat: use build-only to test pull request
```

## Changes in `debian bookworm` since Regolith 3.3

### Changes in [`regolith-control-center`](https://github.com/regolith-linux/regolith-control-center/releases/v1.43.1-9-gnome-43)

```text
c185e43e3 feat: use build-only to test pull request
```

### Changes in [`regolith-session`](https://github.com/regolith-linux/regolith-session/releases/v1.1.14-3-debian-bookworm-debian-bookworm)

```text
40e90f9 feat: use build-only to test pull request
```

### Changes in [`xdg-desktop-portal-regolith`](https://github.com/regolith-linux/xdg-desktop-portal-regolith/releases/v0.3.11-2)

```text
e5ed898 feat: branch on version compatible w/ xdg-desktop-portal v1.18
44463ff Revert "fix: disable newly added gnome xdp interfaces"
4f3cc0f fix: disable newly added gnome xdp interfaces
540ec82 feat: use none instead of regolith backend for disabling interfaces
d0e02f6 fix: dark mode not working (#18)
749f216 fix: remove systemd dep on jammy
2cc2c32 feat: use build-only to test pull request
0bce843 fix: create release tag from correct git sha
254545d feat: enable github action to prepare release
5f17015 feat: enable github action to publish to testing
6f3d57a feat: enable github action to publish to unstable
04be23e feat: enable github action to test pull requests
3d73f37 feat: Implement set_wallpaper_uri method in Wallpaper.Portal (#3)
67214d1 Fix/jammy build (#6)
```

## Changes in `ubuntu jammy` since Regolith 3.3

### Changes in [`i3-wm`](https://github.com/regolith-linux/i3-wm/releases/v4.22-3)

```text
b35b1e80 feat: use build-only to test pull request
```

### Changes in [`i3status-rs`](https://github.com/regolith-linux/i3status-rs_debian/releases/v0.22.2-2-ubuntu-jammy)

```text
d3186f0 feat: use build-only to test pull request
```

### Changes in [`regolith-control-center`](https://github.com/regolith-linux/regolith-control-center/releases/v1.41.20-ubuntu-jammy)

```text
7834d4c07 feat: use build-only to test pull request
```

### Changes in [`regolith-session`](https://github.com/regolith-linux/regolith-session/releases/v1.1.14-3-debian-bookworm-debian-bookworm)

```text
40e90f9 feat: use build-only to test pull request
```

### Changes in [`sway-regolith`](https://github.com/regolith-linux/sway-regolith/releases/v1.7-9-ubuntu-jammy)

```text
d5f17ada feat: use build-only to test pull request
```

### Changes in [`xdg-desktop-portal-regolith`](https://github.com/regolith-linux/xdg-desktop-portal-regolith/releases/v0.3.11-2)

```text
e5ed898 feat: branch on version compatible w/ xdg-desktop-portal v1.18
44463ff Revert "fix: disable newly added gnome xdp interfaces"
4f3cc0f fix: disable newly added gnome xdp interfaces
540ec82 feat: use none instead of regolith backend for disabling interfaces
d0e02f6 fix: dark mode not working (#18)
```

## Changes in `ubuntu plucky` since Regolith 3.3

### Changes in [`regolith-control-center`](https://github.com/regolith-linux/regolith-control-center/releases/v1.48.1-10-gnome-48)

```text
a24afe776 feat: use build-only to test pull request
```
