---
title: "3.5 Release Notes"
linkTitle: "3.5 Release Notes"
weight: 82
description: >
  Release notes for Regolith 3.5
prev: /docs/reference/releases
---

## Regolith 3.5 Release Notes

Regolith 3.5 is a minor release focusing on Ubuntu 26.04 support, broad quality improvements, and closing gaps between the i3 and Sway sessions.

A special thanks in particular to long-time Regolith contributor [cfsmp3](https://github.com/cfsmp3) who has taken initiative to provide the vast majority of fixes and features in the 3.5 release!

The sheer quantity of fixes as well as the depth in testing in this release is primarily due to usage of LLM-based tools by Carlos, myself, and others.  More details about LLM tooling can be found in a recent [blog post on the topic](/posts/llm-tools/).

## Features

* Ubuntu 26.04 (Resolute) support
* Nerd Fonts 3.5.1, with every look moved to Nerd Fonts 3
* Features of the X11 session now in the Wayland session:
  * the volume and brightness indicator on every key press, and the brightness keys on the lock screen
  * the Print screenshot keys
  * Screen Blank and screen lock following Settings, and apps that can lock the screen
  * automount of removable media, the low battery warning and the switch to the power saver profile
  * the "Keep these display settings?" prompt and the monitor number labels of Settings > Displays
  * the compose key and other keyboard options, key repeat, keyboard shortcuts and the cursor theme, taken from Settings
  * IBus and the input source in use, Night Light, and a prompt before logging out, restarting or powering off
* Features of the Wayland session now in the X11 session: the Super+Print screenshot keys, touchpad swipe gestures and the same bar blocks
* New keys in both sessions: back to the previous workspace, focus or launch, stay awake, touchpad on and off, and Night Light
* A folder of pictures, or a GNOME slideshow, can be used as the wallpaper
* Every look sets the GTK color scheme, so apps follow the look's light or dark style
* The `ilia` launcher matches every word of the query in any order, ignores accents, ranks name matches first, and its keybindings page shows the keys the window manager actually binds
* The bar opens Settings from the battery, date and time blocks, shows the keyboard layout in the Wayland session, and colors the CPU, memory and disk values when usage is high

## Platform Changes

* Ubuntu 25.04 (Plucky) is no longer supported, as it has reached end of life.
* Debian 12 (Bookworm) has the X11 session only. `regolith-session-sway` is no longer built for it, as the packages it depends on are not available there.

## Removed Packages

These packages are no longer part of Regolith. Installed copies are not removed by the upgrade.

* `fonts-jetbrains-mono`, `fonts-materialdesignicons-webfont` and `python3-i3ipc`: Ubuntu and Debian ship them, and their packages are used instead.
* `gtklock` on Ubuntu 25.10 and Debian 13: the distribution ships a newer version. Ubuntu 22.04 and 24.04 keep Regolith's package.
* `regolith-unclutter-xfixes`: `regolith-i3-unclutter` now uses the distribution's `unclutter-xfixes`.
* `remontoire`: `ilia` has been Regolith's keybinding viewer since 2.0.
* `armesto`: unused.
* `ubiquity-slideshow-regolith`: Regolith no longer ships an Ubuntu installer.
* `regolith-system-ubuntu` and `regolith-distro-ubuntu` on Debian: they are Ubuntu-only packages that could not be installed on Debian.

## Known Issues

* Nerd Fonts 3 removed the old Material Design icons (`nf-mdi-*`, U+F500 to U+FD46). Custom workspace names or bar labels that use them show as boxes after the upgrade. Use the matching `nf-md-*` icon (U+F0001 and up) from the [Nerd Fonts cheat sheet](https://www.nerdfonts.com/cheat-sheet) instead.
* Regolith now packages 9 Nerd Font families: BitstreamVeraSansMono, Cousine, DejaVuSansMono, Hack, Meslo, Mononoki, RobotoMono, SourceCodePro and Symbols. Other `fonts-nerd-font-*` packages installed from Regolith 3.4 stay installed at version 2.1.0, but receive no further updates.
* `do-release-upgrade` disables the Regolith `apt` line when it upgrades Ubuntu. After the Ubuntu upgrade, re-enable the line with the new codename. See [Upgrade from Regolith 3.4](#upgrade-from-regolith-34).

## Installation Instructions

To install Regolith 3.5 on a new system, follow the [installation instructions](/docs/using-regolith/install/).

### Upgrade from Regolith 3.4

{{< callout type="info" >}}
There is no supported way back from 3.5 to 3.4 other than reinstalling.
{{< /callout >}}

1. Edit the Regolith `apt` line, in `/etc/apt/sources.list.d/regolith.list` if you followed the installation instructions, so that it ends with `v3.5` instead of `v3.4`. For example, on Ubuntu 24.04 (Noble) on `amd64` it should say:

   ```text
   deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg] https://archive.regolith-desktop.com/ubuntu/stable noble v3.5
   ```

   On other releases, the line differs only in the architecture (`amd64` or `arm64`) and in its end:

   | Release | End of the `apt` line |
   |:--------|:----------------------|
   | Ubuntu 26.04 (Resolute) | `https://archive.regolith-desktop.com/ubuntu/stable resolute v3.5` |
   | Ubuntu 25.10 (Questing) | `https://archive.regolith-desktop.com/ubuntu/stable questing v3.5` |
   | Ubuntu 24.04 (Noble) | `https://archive.regolith-desktop.com/ubuntu/stable noble v3.5` |
   | Ubuntu 22.04 (Jammy) | `https://archive.regolith-desktop.com/ubuntu/stable jammy v3.5` |
   | Debian 13 (Trixie) | `https://archive.regolith-desktop.com/debian/stable trixie v3.5` |
   | Debian 12 (Bookworm) | `https://archive.regolith-desktop.com/debian/stable bookworm v3.5` |

1. Update `apt` and upgrade:

   ```bash
   sudo apt update
   sudo apt full-upgrade
   ```

1. Log out and back in, or restart your system.

Regolith 3.4 has no packages for Ubuntu 26.04. If you upgraded to 26.04 with Regolith 3.4 installed, `do-release-upgrade` disabled the Regolith line: make it the Ubuntu 26.04 (Resolute) line above, without a leading `#`, then run the commands of step 2.

After the upgrade is complete, verify that Regolith 3.5 is installed by:

```console
$ cat /etc/regolith/version 
REGOLITH_VERSION=3.5
```

## Changes since Regolith 3.4

### Changes in [`childe`](https://github.com/regolith-linux/childe/releases/v0.1.5)

```text
573cc99 fix: ignore unnumbered and duplicate workspaces when finding the next free one
```

### Changes in [`dracula-gtk`](https://github.com/regolith-linux/dracula-gtk/releases/v4.0.0+git20260824)

```text
0cd4ebe feat: update to upstream 4.0.0+git20260824
```

### Changes in [`fonts-nerd-fonts`](https://github.com/regolith-linux/fonts-nerd-fonts/releases/v3.5.1-2)

```text
efea37f feat: update the fonts to Nerd Fonts 3.5.1
2459f97 feat: package Nerd Fonts 3.5.1
```

### Changes in [`gtklock`](https://github.com/regolith-linux/gtklock/releases/v2.2.3)

```text
b91b502 fix(auth): cap PAM messages and send each in one write
```

### Changes in [`i3-next-workspace`](https://github.com/regolith-linux/i3-next-workspace/releases/v1.0.8)

```text
1a4de1d fix: quote workspace names in the i3 command and fail when i3 rejects it
c4219f1 fix: work without workspace-name resources and past workspace 49
0bff36c fix: accept workspace names that contain a colon
```

### Changes in [`i3-snapshot`](https://github.com/regolith-linux/i3-snapshot/releases/v1.1.1)

```text
41f7b5b fix: record floating windows and restore windows whose workspace is gone
bcc6d15 fix: resolve cmake 4 versioning issue introduced in resolute
```

### Changes in [`i3-swap-focus`](https://github.com/regolith-linux/i3-swap-focus/releases/v0.4.6)

```text
b0b4c62 fix: remove the PID file when the daemon is stopped by a signal
```

### Changes in [`i3status-rs`](https://github.com/regolith-linux/i3status-rs_debian/releases/v0.32.1-3)

```text
5e5beb6 fix: address clippy findings of current Rust
4c04bf2 style: format bluetooth.rs with rustfmt
802c69f fix: address clippy findings of current Rust
06b2a87 style: format bluetooth.rs with rustfmt
3a99153 fix(keyboard_layout): skip sway events for virtual keyboards
980fc05 feat: open Settings from the battery, date and time blocks
bca844f feat: show the keyboard layout in the sway bar
95ba7b9 fix: hide the rofication block when Rofication is not installed
```

### Changes in [`i3xrocks`](https://github.com/regolith-linux/i3xrocks/releases/v1.3.7)

```text
435655b fix: do not crash on an undefined xresource without a default
```

### Changes in [`ilia`](https://github.com/regolith-linux/ilia/releases/v0.20.1)

```text
4e22222 fix(launch): name the scope of a command line after its program
e96a4dd fix(a11y): don't crash when an AT-SPI client leaves during a key press
eca4426 feat(help): make help a page that scrolls, in single and all-pages mode
6d60ca4 docs: drop the README section on window manager keys
b6ad6df fix(apps): show an app grid as the icon of the Applications page
a960161 fix(filter): build with the GLib bindings of older Vala releases
c6b2e25 fix(keys): let window manager and desktop keys work while ilia is open
3d18054 feat(filter): rank name matches first, ignore accents, optional fuzzy matching
1723b18 fix(filter): show no page icon in the entry after a tab switch
ee43f9a feat(keybindings): show the keys the window manager actually binds
a5064e3 fix(apps): launch desktop entries as the Desktop Entry spec says
dffa6bc fix(wayland): stay open when the seat's last keyboard goes away
0585dbe fix(windows): log no critical for native Wayland windows
4f7b01c Apps page: look themed icons up by their plain name
da2aaaa Apps page: address review of the early-startup changes
b600991 Apps page: read desktop files during GTK startup, decode icons off the main thread
236f5af fix(keybindings): send the command the window manager holds for the binding
affbb11 fix(keybindings): read the config files in order, each on its own lines
56562ce fix(keybindings): run the clicked row's binding on X11
c6f236c fix(keybindings): run the chosen binding over IPC, quotes intact
ba562cf fix: paste without blocking on the clipboard owner
bab28e4 fix(filter): highlight the first match for new text after arrow keys
c30a10e fix(commands): list each command once, and only commands
532cc78 fix(files): find files by their content
aee52af fix(notifications): highlight the new last row after deleting the last one
0aeac3a fix(keys): count entry positions in characters for word motion and copy
9d0b346 fix(keys): close on the right Super key too
3b7e546 fix(apps): search keywords without a comment, and the generic name
3083289 fix(apps): show no black square for apps without an icon
7481ac8 fix(windows): keep the icon column when a window has no icon
c2289e5 fix(windows): list dialog windows on i3, as on sway
e30408e fix(filter): apply the entry text to the switched page
4d819de fix(x11): close before a suspend so the screen locker can lock
7bf89fe fix(tracker): search files without blocking the UI
f0bc483 fix(keys): keypad Enter acts on the highlighted row, like Return
0789ebe fix(wayland): detect Wayland from the GDK display
725df67 fix(ipc): read the sway socket from SWAYSOCK
c4c534f fix(wayland): Alt+minus shrinks the window right away
67aa7ff fix(notifications): do not hang on a daemon that never answers
8e1ea20 fix(notifications): Delete removes the highlighted notification
222a0ed fix(keys): move with Ctrl+n/p from the highlighted row
9e1f71d fix(keys): switch keyboard layout while the launcher is open
aca531c fix(textlist): fill the view in bounded steps, stop when the window closes
d64843a fix(textlist): stay responsive on large lists
3b5f02a fix(notifications): list the newest notification first
8d03dc5 fix(files): load tinysparql or tracker-sparql at run time
bdea8ec fix(apps): run Terminal=true apps in a terminal again
28e0359 feat(keys): Home and End select the first and last row
22fc722 feat(filter): match every word of the query, in any order
e35bb60 fix(keys): let Enter accept typed text that matches no item
1911f87 fix(ipc): read the whole i3/sway IPC reply
f4887df fix(apps): load icons on the main thread to stop model race
```

### Changes in [`lago`](https://github.com/regolith-linux/lago/releases/v0.2.0-3)

```text
a467d64 fix(debian): drop the -dev runtime dependency and clean up packaging
```

### Changes in [`libtrawldb`](https://github.com/regolith-linux/libtrawldb/releases/v0.1.1-1)

```text
85d9b13 fix: remove proxy_new() from client_api.h
6cf504e fix: don't crash in conf_client_init() when err is NULL
```

### Changes in [`nordic`](https://github.com/regolith-linux/nordic/releases/v2.2.0+git20260723-1)

```text
fe8ba67 feat: update to upstream 2.2.0+git20260723
```

### Changes in [`regolith-avizo`](https://github.com/regolith-linux/avizo/releases/v0.1.8)

```text
2b857f6 fix(service): let the latest Show() decide when the notification hides
cc184ca fix(debian): don't fail the configure when light is missing
cf6bd4f fix(client): exit with the status of the command line
501a238 fix(volumectl): accept -M and -h after the command
7216bbb fix(packaging): build regolith-avizo as Architecture: any
bac30e9 fix: report version 1.3
bc121dc feat: add a focused-output setting to show the OSD on the focused output only
884406e fix: show the image that each call asks for
0c382ad fix(service): start on demand over D-Bus, exit if already running, quiet log
1ad2ee1 fix: draw the border correctly when border-radius is smaller than border-width
4631afb feat: add a show command to volumectl and lightctl
de5ffaf fix(volumectl): stop at 100% unless -b is given, and make -b a plain flag
c7314f5 fix: hide the OSD after 1.5 seconds, as the X11 session does
22b33ff fix: draw the OSD images sharply on scaled outputs
4624e59 fix(volumectl): show the OSD when the device is not muted
2a105c7 fix broken link
95ca837 fix options of lightctl
090f8a5 fix options of lightctl
ffa2aad Add error message for when avizo-service is not running
b296d98 Fix move calls
77e5b01 Add x_offset, heyjuvi#70
d0c9f5f Fix typo, aquired -> acquired
b59181e Fix #48 bashism in lightctl
b459e41 update scripts for monitor option
5ff32ea add monitor option
28955b7 Fix indentation for setting namespace
78198b1 Set namespace to avizo for gtk-layer-shell
4b9f951 Format volumectl
01a5b5a Format lightctl
b3db209 Format avizo_service.vala
d489f84 Format avizo_client.vala
3484523 added dark variants to listed resources
1c11616 added -d flag to the help menu
6f1087b fix volumectl
51cd8ab add -d flag for dark theme
4ea9ec3 added dark theme icons
```

### Changes in [`regolith-compositor-compton-glx`](https://github.com/regolith-linux/regolith-compositor-compton-glx/releases/v1.2.4)

```text
3dcbe0b fix: quote the user compton config path and exec compton
fa4d915 fix: point the init comment at the current compositor docs
8b87500 fix: replace the regex shadow exclusion that makes compton exit at start
```

### Changes in [`regolith-compositor-none`](https://github.com/regolith-linux/regolith-compositor-none/releases/v1.0.5-1)

```text
bc70506 fix: point the init comment at the current compositor docs
```

### Changes in [`regolith-compositor-picom-glx`](https://github.com/regolith-linux/regolith-compositor-picom-glx/releases/v1.4.1-1)

```text
862ebde fix: point the init comment at the current compositor docs
962a8fc fix: replace only this user's picom in init
3b23e7e fix: use the xrender backend on software renderers
cc1bce6 fix: start picom again after it crashes
ff9f7f8 test: install EGL for the picom-config autopkgtest
703562e fix: keep the config free of deprecated options on picom 12 and 13
```

### Changes in [`regolith-compositor-xcompmgr`](https://github.com/regolith-linux/regolith-compositor-xcompmgr/releases/v1.4.1-1)

```text
4a440f5 fix: point the init comment at the current compositor docs
```

### Changes in [`regolith-control-center`](https://github.com/regolith-linux/regolith-control-center/releases/v1.46.0-6-gnome-46)

```text
3c959f99c fix: ship panel desktop files and load them with a regolith- prefix
e64fb6cca region: store the formats in accountsservice too, for Ubuntu's session
55742395c background: show the chosen wallpaper in sway without reloading it
e66501083 keyboard: also read keybindings from gnome-control-center/keybindings
```

### Changes in [`regolith-default-settings`](https://github.com/regolith-linux/regolith-default-settings/releases/v2.0.7)

```text
4057189 fix(debian): conflict with session-shortcuts
87992c3 fix: stop offering the logout, reboot and shutdown entries as terminals
aa60b0e fix: give the sway session the same GNOME defaults as X11
39d8cfe fix: ship the schema override from the source tree, not from debian/
2e4edc8 test: check the session action entries in both Regolith sessions
7709cba fix: add logout, reboot and shutdown entries for the sway session
081668b fix: only show session actions on X11 due to incompat w/ wayland.
```

### Changes in [`regolith-displayd`](https://github.com/regolith-linux/regolith-displayd/releases/v0.6.0)

```text
745e18f fix: find the home directory without $HOME instead of panicking
6390f64 fix: refuse layouts with extreme positions instead of overflowing
ba22476 test: check the kanshi include on every kanshi version and its directory
d1698bc fix: write the kanshi include in the syntax of the installed kanshi
5ff22b4 test: check the kanshi configuration for home directories with spaces and quotes
c28fd23 fix: quote paths in regolith-displayd-init and the kanshi include
331545e test: don't run the fake kanshi scripts while another child holds them open
43fbecd ci: check formatting, clippy and tests on every pull request
93264f9 fix(tests): don't exit silently when kanshi has already exited in display-config-headless
447527d refactor: use wayland-client for the Night Light gamma tables
bca50d7 feat: apply Night Light in the Wayland session
b2033ca feat: show monitor number labels for Settings > Displays in sway
1154862 fix: address clippy warnings
d50c60c style: apply cargo fmt
fa28955 fix: match known modes by monitor identity, not full equality
581c53f fix: compare and hash monitors by the same fields
6a49c54 fix(tests): make the exiting fake kanshi a script, not a copy of sleep
7b6192d test: don't let display-config-headless exit silently
b0db523 feat: ask to keep display changes, and revert them after 20 seconds
ed5439f fix: report and remember the primary display
30e5482 fix: don't save temporary display changes
021c5e6 fix: keep the advertised display properties across Apply
fb5ecf8 fix: refuse display configurations that Mutter refuses
601495f fix: offer the scales Mutter offers for each mode
1e86115 fix: keep connector-named kanshi profiles to the monitors they were saved for
cec4d80 fix: refuse display configurations made for other monitors
5d05992 fix: tell identical monitors apart in kanshi profiles
2e4a54c fix: name monitors like Mutter and stop reporting pixels as millimetres
a23d3e7 test: wait for the fake kanshi to be running before reloading
634af4b test: add an autopkgtest for the displayd and kanshi user units
c70dd92 fix: keep regolith-displayd running when kanshi restarts
8a965dc style: format the kanshi reload code and simplify a map_or
77d2d0e fix: restart kanshi when it cannot reload
080a639 test: add a headless sway autopkgtest for the DisplayConfig API
a6f9148 fix: apply kanshi profiles reliably
b285552 fix: report the current mode for outputs with no mode list
edad3ac fix: display config doesn't apply with disabled outputs
```

### Changes in [`regolith-distro-ubuntu`](https://github.com/regolith-linux/regolith-distro-ubuntu/releases/v2.0.1-1)

```text
d5de4fe fix(debian): make regolith-distro-ubuntu a transitional package
```

### Changes in [`regolith-ftue`](https://github.com/regolith-linux/regolith-ftue/releases/v2.2.4)

```text
958f6dd fix: don't print an error when XDG_SESSION_TYPE is unset
bf610a2 fix: depend on libglib2.0-bin for gsettings
01ac652 fix: quote terminal profile strings as GVariant and retry failed setups
fdae125 fix: read the terminal profile setting from trawl in the sway session
6703051 fix: show the first-login keybinding help with the active look's stylesheet
```

### Changes in [`regolith-i3xrocks-config`](https://github.com/regolith-linux/regolith-i3xrocks-config/releases/v6.0.1)

```text
a54c8de fix: keep media and window titles with line breaks on one bar line (#185)
9422729 fix(debian): make i3xrocks-cpu-usage depend on perl (#184)
102ed03 fix(updates): make i3xrocks-updates installable and working on Debian (#182)
7ec733f fix(debian): recommend tailscale instead of depending on it (#181)
d6a2089 fix(updates): use the Nerd Fonts 3 code point for the default label (#176)
3cbd253 fix(temp): fall back to other CPU sensors instead of dividing by zero (#178)
e2975b2 fix(weather): show the error message for wttr.in error replies (#177)
46db41f Fix microphone blocklet on PipeWire systems (#167)
e788883 fix(volume): stop at 100% when scrolling up (#175)
972f30f fix(debian): recommend gkbd-capplet for the keyboard layout blocklet (#174)
0138a6c fix: show bluetooth device names and Wi-Fi SSIDs as they are (#173)
43244c6 fix(key-indicator): hide the off indicators again (#172)
dc4660d fix: open System Monitor again from the CPU and memory blocklets (#171)
145f5f9 fix(time): show AM or PM with the 12-hour clock (#170)
80fc406 fix(memory): show MemAvailable as free memory (#169)
cf0d9b6 feat: colour the CPU, memory and disk values when usage is high (#168)
8c7c3ed fix(debian): make i3xrocks-info depend on ilia, not remontoire (#166)
2e80d3e battery: accept kernel battery names as the instance (#165)
138c933 nm-vpn: do not report bridge-attached tun/tap devices as a VPN (#164)
b12c99b key-indicator: describe what the blocklet shows (#163)
4242391 weather: collapse the padding whitespace from wttr.in (#162)
6da3250 Fix volume blocklet on PipeWire systems (#161)
1d9aadc fix: align ilia launch commands with wm config (#160)
```

### Changes in [`regolith-inputd`](https://github.com/regolith-linux/regolith-inputd/releases/v0.6.1)

```text
1b9c732 fix: start the IBus panel again for an output created again under its name
048726b fix: make IBus opt-in and restart its Wayland panel on new outputs
57fc0e7 fix(debian): replace the dh_make placeholders in Section and Homepage
93dd516 feat: follow GNOME's input-source switching rules in sway
023378d feat: run IBus in the sway session and follow the input source in use
25223b1 feat: mirror the Sound and Media and System shortcuts from Settings to sway
8f97468 fix: put the start limit in the [Unit] section
7a77e8e fix: apply input source variants with their layouts, skip unusable sources
4e44ae4 fix: keep running with input devices that have no vendor or product id
5e87644 fix: send sway's name for the touchpad's disabled-on-external-mouse
a4b3b23 fix: apply mouse handedness to touchpads that follow the mouse
3a8fb5e docs: list the media-keys shortcuts that are not mirrored yet
d622cd4 fix: apply keyboard repeat at startup, honour repeat off and the acceleration profile
4bc519e fix: skip built-in shortcuts the media-keys schema does not have
d162d30 feat: mirror GNOME keyboard shortcuts to sway
0aac33f feat: mirror GNOME's cursor theme and size to sway
141a19a feat: mirror GNOME's XKB options (compose key and others) to sway
de2514a fix: apply the touchpad click method at startup and when it changes
f2cedb3 fix: keep running when no input source is set in GNOME
cbb4afc test: run cargo test at package build and add an integration harness
4c654ef WIP: link gsettings click-method to sway click_method.  Address https://github.com/regolith-linux/regolith-desktop/issues/1076
```

### Changes in [`regolith-lightdm-config`](https://github.com/regolith-linux/regolith-lightdm-config/releases/v1.4.0)

```text
e40bfc5 fix: name the greeter font by its Nerd Fonts 3 family
817bd44 fix: keep slick-greeter as the greeter when lightdm-gtk-greeter is installed
```

### Changes in [`regolith-look-default`](https://github.com/regolith-linux/regolith-look-default/releases/v0.9.0)

```text
24cab0c feat: move the default look to Nerd Fonts 3
8abb21b fix(loader): name the lockscreen wallpaper path in its diagnostic
8dea0e4 fix(loader): percent-encode wallpaper paths in file URIs
60d612f fix(loader): set picture-uri-dark along with picture-uri
59982de test: check every look sets a valid gtk.color_scheme at build time
551c2c4 feat(default): set gtk.color_scheme to prefer-dark
454355d Ensure GNOME supports color-scheme
f309320 Add support for org.gnome.desktop.interface color-scheme
```

### Changes in [`regolith-look-extra`](https://github.com/regolith-linux/regolith-look-extra/releases/v0.11.0)

```text
ae804ab feat: move every look to Nerd Fonts 3
e6c4561 fix(gruvbox): keep the sway lock screen clock readable
5671836 fix(i3-default): depend on the DejaVuSansMono Nerd Font the look uses
8559dc9 fix(dracula): depend on the Dracula icon theme instead of Moka
4cf2ac3 test: check the looks' picom configs at build time
451c3ef fix: keep the looks' picom configs free of deprecated options
958ea4d fix(nevil): use a proportional UI font
0545f27 test: check every look sets a valid gtk.color_scheme at build time
51a999b feat: set gtk.color_scheme in every look
```

### Changes in [`regolith-powerd`](https://github.com/regolith-linux/regolith-powerd/releases/v0.10.0)

```text
a2ddd8a fix: lock with regolith-sway-gtklock, as the lock key does
41de410 feat: pass the Displays panel's monitor labels on to regolith-displayd
93acedb fix: depend on bc, which the idle dim uses
869093d fix: put the start limit in the [Unit] section
2803cdc fix: don't poll the session inhibitors every second with Screen Blank set to Never
224c60d fix: dim and turn off the screen at GNOME's times
0bdb079 build(debian): depend on the media-handling schema, recommend gvfs and gcr
c512243 feat: mount removable media automatically in the sway session
6733b9d feat: confirm logout, restart and power off requests in the sway session
fda38bc feat: switch to the power-saver profile when the battery is low
c5a3ccb fix: turn the screen off soon after it is locked
0076459 fix: let explicit suspend through while an app inhibits suspend
ebefedb fix: apply the new power source's idle action after plugging in or unplugging
4740916 fix: don't hang when two power settings change at once
340951e fix: lock the session when the lock-screen wallpaper path has spaces or ~
b400865 fix: honour the lockdown setting disable-lock-screen in sway
0c354ec fix: keep the power button bound after a sway reload
9f719c3 fix: make the "blank" sleep-inactive type turn the outputs off
1827347 refactor: read the screen-lock settings through gen_settings
7b4a960 fix: honour the GNOME idle, dim and screen-lock settings in sway
4bc5f11 feat: warn when the battery is low
3f9ee2b fix: keep the screen from staying dimmed or off after a reload or a lock
f817791 feat: provide org.gnome.ScreenSaver in the sway session
872649e fix(session): release locks on SessionManager loss and reconnect on system-bus closure
277ed1a refactor(logind): model inhibitor slots as Lock and rename session helpers
c4bf299 feat(session): share session settings and cap poll interval
5647283 refactor: tidy duplicated strings, imports and warnings
c886068 fix: treat an unknown power source as AC, not battery
d3b9619 feat(session): mirror GNOME session inhibition onto logind locks
90c4b12 refactor(logind): acquire the startup inhibitor asynchronously
```

### Changes in [`regolith-rofication`](https://github.com/regolith-linux/regolith-rofication/releases/v1.6.0)

```text
b34636e fix: take everything after the first colon as a socket request's argument (#36)
5f65ef8 fix: keep notifications whose urgency hint is out of range or not a number (#35)
2e0908e fix: save the queue when the daemon's socket was removed (#33)
ee2a469 fix: don't reuse notification IDs after a restart (#32)
c982323 fix: advertise persistence and keep transient notifications out of the saved queue (#31)
9522216 fix: declare the Notify hints as a{sv} (#30)
d167556 fix: emit NotificationClosed when a notification is closed or dismissed (#29)
9bb7b8f feat: make the rofication-gui key bindings configurable (#27)
6080695 feat: make the single-notification app list configurable (#26)
54d9e8f fix: save the notification queue when the daemon is stopped by a signal (#28)
bcb0c11 fix: do not crash rofication-gui when xrescat is missing (#25)
```

### Changes in [`regolith-session`](https://github.com/regolith-linux/regolith-session/releases/v1.3.1-1)

```text
50082f1 fix: end the X11 session at logout on gnome-session 48 and 49 (#109)
3cd7085 fix: make regolith-diagnostic report the installed sessions from a console (#106)
4e3fa9a fix(tests): check upgrade relations without changing the installed packages (#105)
a48cb84 feat(debian): show the same bar blocks in the X11 session as in sway (#87)
36be167 Chore/release 1 2 5 (#104)
8aec779 fix: rebuild the resources from scratch on regolith-look set and refresh (#98)
ab562b7 fix: keep xdg-terminal-exec from picking foot in the X11 session (#102)
94c35d4 fix: make regolith-diagnostic report sway in the Wayland session (#100)
d0fe095 fix: end the Wayland session when sway exits, and apply the look once there (#99)
c0e389e fix: keep i3 restartable when the session runs gnome-session --builtin (#95)
2159f67 fix: run the post-logout script when the session ends (#96)
12de6c4 fix: make regolith-look refresh merge the X resources in the login order (#93)
7b9585a fix: keep an earlier session's SWAYSOCK and I3SOCK out of the next one (#91)
a16f836 fix(debian): depend on gnome-session-common, which ships the session targets (#89)
50b8f31 fix(debian): drop the unused clipman dependency (#79)
d677727 fix: give the Wayland session its desktop names
f9c6902 fix: keep non-ASCII text in Xresources values (#85)
90ea509 fix: tell Java applications that sway does not reparent windows (#83)
6704b6c fix(debian): let plain apt upgrade move the sway session forward (#84)
fdc3c5a fix: keep dead keys working in X11 applications in the Wayland session (#82)
5b1a945 fix: keep the PATH from ~/.profile when gnome-session runs the session itself (#80)
da0b6d2 fix: take the system keyboard layout on a first Wayland login (#78)
33edfbb fix: recommend regolith-default-settings from the sway session (#73)
7331350 fix: report a missing look instead of an unbound variable error (#75)
06849f5 fix: honour Xft.dpi, apply the look once, keep the default look across look updates (#72)
9efa20e fix: do not open GNOME Initial Setup in the Regolith sessions (#70)
4b565a6 fix: hide the distro picom autostart entry in the Regolith sessions (#71)
bd7b183 fix: keep the X11 lock screen's context menu closed from the keyboard (#69)
f602885 fix: clean up workspace names at X11 login as regolith-look refresh does (#68)
267780a fix: start the GNOME Flashback polkit agent from autostart where it has no unit (#66)
462b87a X11 session on Ubuntu 26.04+: start the polkit agent and idle monitor, handle session failures (#64)
b742843 fix: add missing i3-wm dependency (#61)
```

### Changes in [`regolith-system-ubuntu`](https://github.com/regolith-linux/regolith-system-ubuntu/releases/v1.1.0-1)

```text
9f1b782 Rename the OutputClass identifier to "vc4"
6708962 Only ship the Raspberry Pi Xorg config in the arm64 package
ab3e2ea feat: make lightdm work on Raspberry Pi out of the box
bf5045b fix(debian): recommend plymouth-theme-regolith-logo, the theme's real name
```

### Changes in [`regolith-wm-config`](https://github.com/regolith-linux/regolith-wm-config/releases/v4.13.1)

```text
75fb0a4 fix(debian): stop regolith-session-sway from pulling in i3-wm and xdotool
75035a3 chore(debian): remove the orphaned regolith-sway-touchpad-guestures files
fb6826f fix(sway): accept colours with alpha (#RRGGBBAA) in looks
d752585 fix(clamshell): compare the display count as a number
a1fd1f3 fix(debian): stop building the sway packages for Debian 12
6ff06d3 fix(sway): keep the volume keys working with an avizo without volumectl -n
86a9a0c fix(sway): name the audio device in the volume OSD, as X11 does
27a749c fix(sway): show the look's lock-screen colours and image placement
373237d fix(sway): end the style watcher when sway exits
ba20813 Revert "feat: add focus-or-launch, pop-out and transparency keys to both sessions"
ac91d3f fix(debian): break regolith-powerd older than 0.9.0 in regolith-sway-session
f78febd fix(sway): ask before logout, reboot and power off through the end-session dialog
c0c39c7 test(i3): skip workspace-memory where the dummy X driver has no outputs
5f116a7 test(i3): add an autopkgtest for regolith-i3-workspace-memory
6eb45a2 feat(i3): move workspaces back to a monitor when it returns
9c5323a refactor: rename regolith-i3-wallpaper to regolith-i3-wallpaper-folder
bdf587e feat: show a folder of pictures as the wallpaper, and play GNOME slideshows in sway
fbfc2ae feat: add focus-or-launch, pop-out and transparency keys to both sessions
d25ede8 docs(debian): shorten the regolith-wm-quick-toggles description
e11d84f feat: add keys for stay awake, the touchpad and Night Light in both sessions
47d8155 fix(sway): handle the Eject, Battery and airplane mode keys
506e730 feat(sway): let a look style the gtklock lock screen
d4c5d11 fix(sway): show the other picture of a wallpaper pair on a style switch
dd5e6e6 fix(sway): handle the Rewind, Forward, Repeat, Shuffle and Media keys
3880091 fix(sway): read the look selector icon from trawl in the sway session
898b509 fix(clamshell): run the lid close action when there is no eDP panel
5c30ccd fix(debian): use the distribution's unclutter-xfixes for the i3 session
ae5d3e2 feat(i3): add touchpad swipe gestures like the sway session's
2823090 fix(sway): also open the calculator when it is installed as a snap
c2ff4d4 feat(i3): add sway's three Super+Print screenshot keys to the X11 session
6914c0f fix(sway): handle the launcher and lock keys as the X11 session does
6a2056b fix(sway): add the fine volume step and the media key variants of X11
3e4f294 fix(sway): don't show the config error bar for Alt+Print when wm.mod is Mod1
d0512d8 fix: apply the configured border styles to tiled and floating windows
3927eea fix(sway): load the config when a look lacks child-border colours or ilia.stylesheet
8eb5a5c fix(sway): make Super+= and Super+Shift+= widen the gaps as in i3
1cec334 fix(sway): apply wm.bar.trayoutput to the bar tray
22ce167 fix(sway): lock the session when the lock-screen wallpaper path has spaces or ~
28a474f fix(i3): stop the old compositor when another compositor package replaces it
0d920e9 fix: open the Look Selector with the look's ilia stylesheet
ba1b4fb fix: make Up and Down resize the same way as k and j
10551d0 fix(sway): draw SVG wallpapers instead of taking them for GNOME XML files
bf69394 fix(sway): float native Wayland windows with the app_id floating_window
20297b2 fix(sway): draw the wallpaper colours and gradients as the X11 session does
3e3add9 fix(sway): read wm.hide_cursor.timeout, keeping the old timout spelling
c622c56 feat: let Xresources change what the mouse wheel does over the bar
0325c01 feat: add a key to switch back to the previous workspace
092156f fix(sway): reload kanshi on sway reloads instead of restarting it
481c262 feat(sway): let users opt out of the XWayland settings service
0e2ff1f feat(sway): use the X11 session's screenshot keys
27248b9 fix(clamshell): take the lid close action from systemd-logind, as X11 does
77cdd52 fix(sway): serve the GNOME theme, fonts and cursor to XWayland apps
49c2477 fix(sway): keep the brightness keys working on the lock screen
65ba3d7 fix(debian): bring back regolith-sway-audio-idle-inhibit as a transitional package
3eac9be fix(debian): install the i3 next-free-workspace bindings again
34d82d8 fix(sway): make Terminate App work for native Wayland windows
b17224c docs(i3): list the screenshot keys in the keybinding viewer
5ecc2a8 fix(sway): keep the user's input method choice across reloads
6454b0f docs(i3): list the layout-switch keys in the keybinding viewer
ff7a300 fix(debian): make picom-glx the X11 compositor default for every solver
8ccd127 fix(debian): restore the settings integration after a release upgrade
1f24705 fix(sway): give the previous-layout binding its own resources
a6bc75c fix(sway): allow switching keyboard layout on the lock screen
d8b60b1 fix: stop hard-coding Mod1 so the wm.mod/wm.alt swap works
bd4854d fix(clamshell): treat an unknown power source as AC, not battery
e7d2748 fix(clamshell): lock the session before suspending on lid close
5379e75 fix: publish compositor env to gnome-session before READY
```

### Changes in [`sway-audio-idle-inhibit`](https://github.com/regolith-linux/SwayAudioIdleInhibit/releases/v2.0.5)

```text
1f583e4 fix: leave room for the end of the ignored source outputs list
6498b26 fix: re-enable the service when the stale link was removed by hand
9ac52f4 fix: don't abort when the first inhibit comes after the bus auth timeout
aca5908 fix: run only in the Regolith Wayland session
c327047 fix: remove stale systemd link from upgrade from 25.10 to 26.06
b36a335 fix: fix target to remove cycle present in ubuntu 26.04 gdm3.  Fixes gdm3 breaking when regolith is installed
```

### Changes in [`trawl`](https://github.com/regolith-linux/trawl/releases/v0.3.1)

```text
c888f91 fix(tests): give each trawldb integration test its own bus and trawld
a379169 fix(debian): version 0.3.0 so Regolith's trawl outranks the distribution builds
fed74fd fix(debian): make trawld depend on cpp
b01ae48 Revert experimental pkgforge/CI changes from 2026-05-10..11
01aea09 fix: remove and replace unstable api usage to support jammy and noble
a45d2ef fix(trawldb): edit values into the file with --edit, like xrdb -edit
67f7a73 fix(trawldb): exit 1 when a command fails
0187f93 fix(trawld): keep restarting trawld after occasional crashes
f2a08c1 fix(trawldb): make resources() read the Resources property
b45285c fix(trawldb): drop quit() from the D-Bus proxy
c3845bc fix(trawldb): clear resources before --load/--merge with --remove
3464b96 fix(trawld): exit 0 for --help and --version
b6c170b fix: don't delete the resource file when -I/-D arguments contain spaces
39b50d5 bump: revision to 5 for re-upload
7b5ed01 bump revision to 4
ce51077 remove install_doc config
9cbd346 bump revision to 3
87b094d bump revision to 2 for re-upload
4adbba2 fix: configure git user for install-doc step
830f346 specify container image for pkgforge-runner
8a09040 feat: add action
b8b019d exp: new package build test
b46b38e Revert "Add pkgforge configuration for multi-distro packaging"
61eafba Add pkgforge configuration for multi-distro packaging
eb3906b fix: rebased changelog
da78cae fix: fix target to remove cycle present in ubuntu 26.04 gdm3.  Fixes gdm3 breaking when regolith is installed
```

### Changes in [`whitesur-gtk-theme`](https://github.com/regolith-linux/WhiteSur-gtk-theme/releases/v1.2-2026.09.10)

```text
cc66cf9 fix: point Homepage and copyright Source at the upstream project
1e3c367 feat: update to upstream release 2026-09-10
```

### Changes in [`xdg-desktop-portal-regolith`](https://github.com/regolith-linux/xdg-desktop-portal-regolith/releases/v0.4.0-1)

```text
1bb2f04 feat: retire the portal backend (#39)
c01a60f feat: expose the accent colour through the Settings portal (#38)
332bbe0 fix: route X11 Screenshot to the GNOME portal and disable ScreenCast (#34)
b9c6d72 fix: drop libjsonrpc-glib-1.0-dev from the runtime Depends (#33)
```

## Changes in `debian bookworm` since Regolith 3.4

### Changes in [`regolith-control-center`](https://github.com/regolith-linux/regolith-control-center/releases/v1.43.1-10-gnome-43)

```text
a6f6aabcd fix: ship panel desktop files and load them with a regolith- prefix
```

### Changes in [`regolith-session`](https://github.com/regolith-linux/regolith-session/releases/v1.1.15-1-debian-bookworm-debian-bookworm)

```text
299d1fe fix: rebuild the X resources from scratch on regolith-look set and refresh (#101)
548fb47 fix(tests): check this source's packages, not the testbed, in sessions-per-vendor (#103)
cad1114 fix: stop building the sway session for Debian 12 (#86)
78a9371 fix: make regolith-look refresh merge the X resources in the login order (#94)
72ff011 fix: keep an earlier session's SWAYSOCK and I3SOCK out of the next one (#92)
c4cda27 fix(debian): depend on gnome-session-common, which ships the session targets
c79f6ab fix: keep the PATH from ~/.profile when gnome-session runs the session itself (#81)
a6c076a fix: recommend regolith-default-settings from the sway session (#74)
7cdbbdf fix: keep i3 restartable when the session runs gnome-session --builtin (#65)
```

### Changes in [`xdg-desktop-portal-regolith`](https://github.com/regolith-linux/xdg-desktop-portal-regolith/releases/v0.4.0-2)

```text
b8881fe feat: retire the portal backend (#40)
aee8576 fix: stop the deprecated backend from answering file chooser and other portals on jammy and bookworm (#36)
```

## Changes in `debian trixie` since Regolith 3.4

### Changes in [`i3status-rs`](https://github.com/regolith-linux/i3status-rs_debian/releases/v0.22.2-3-ubuntu-jammy)

```text
70790b9 feat: open Settings > Date & Time from the clock (v0.22)
e2e225a fix: hide the rofication block when Rofication is not installed
```

### Changes in [`regolith-control-center`](https://github.com/regolith-linux/regolith-control-center/releases/v1.48.1-11-gnome-48)

```text
5b1c41d37 fix: load the regolith- panel desktop files this package ships
```

## Changes in `ubuntu jammy` since Regolith 3.4

### Changes in [`i3status-rs`](https://github.com/regolith-linux/i3status-rs_debian/releases/v0.22.2-3-ubuntu-jammy)

```text
70790b9 feat: open Settings > Date & Time from the clock (v0.22)
e2e225a fix: hide the rofication block when Rofication is not installed
```

### Changes in [`regolith-control-center`](https://github.com/regolith-linux/regolith-control-center/releases/v1.41.21-ubuntu-jammy)

```text
24860828b fix(debian): make regolith-control-center-dev depend on the package with the keybindings
ceced8209 fix: load the regolith- panel desktop files this package ships
37bbeb887 keyboard: also read keybindings from gnome-control-center/keybindings
```

### Changes in [`regolith-session`](https://github.com/regolith-linux/regolith-session/releases/v1.1.15-1-debian-bookworm-debian-bookworm)

```text
299d1fe fix: rebuild the X resources from scratch on regolith-look set and refresh (#101)
548fb47 fix(tests): check this source's packages, not the testbed, in sessions-per-vendor (#103)
cad1114 fix: stop building the sway session for Debian 12 (#86)
78a9371 fix: make regolith-look refresh merge the X resources in the login order (#94)
72ff011 fix: keep an earlier session's SWAYSOCK and I3SOCK out of the next one (#92)
c4cda27 fix(debian): depend on gnome-session-common, which ships the session targets
c79f6ab fix: keep the PATH from ~/.profile when gnome-session runs the session itself (#81)
a6c076a fix: recommend regolith-default-settings from the sway session (#74)
7cdbbdf fix: keep i3 restartable when the session runs gnome-session --builtin (#65)
```

### Changes in [`xdg-desktop-portal-regolith`](https://github.com/regolith-linux/xdg-desktop-portal-regolith/releases/v0.4.0-2)

```text
b8881fe feat: retire the portal backend (#40)
aee8576 fix: stop the deprecated backend from answering file chooser and other portals on jammy and bookworm (#36)
```

## Changes in `ubuntu noble` since Regolith 3.4

### Changes in [`xdg-desktop-portal-regolith`](https://github.com/regolith-linux/xdg-desktop-portal-regolith/releases/v0.4.0-3)

```text
3a49f7e feat: retire the portal backend (#41)
e3869fb fix: disable the interfaces routed to the deprecated backend on noble (#37)
8f71e1c fix: route X11 Screenshot to the GNOME portal and disable ScreenCast on noble (#35)
de8e292 fix: dark mode not working in the X11 session on noble (#32)
```

## Changes in `ubuntu questing` since Regolith 3.4

### Changes in [`regolith-control-center`](https://github.com/regolith-linux/regolith-control-center/releases/v1.49.0-5-gnome-49)

```text
d2aee238e fix: install the Graphics Tablet panel desktop file too
323104614 test: validate the panel desktop files at build time
f027652f0 fix: ship panel desktop files and load them with a regolith- prefix
ae2e1343b region: store the formats in accountsservice too, for Ubuntu's session
213d759dd fix: show the Graphics Tablet panel again
```
