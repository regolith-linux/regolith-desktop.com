---
title: "Test What's New in Regolith 3.5"
linkTitle: "Test What's New in 3.5"
description: >
  Try the user-facing fixes and new features of Regolith 3.5 yourself, in the X11 and Wayland sessions
---

This page lists the changes from Regolith 3.4 to 3.5 that you can see or feel in normal use, with the steps to try each one yourself. Internal fixes (packaging, build, quoting and the like) are left out.

{{< callout type="warning" >}}
Regolith 3.5 is not released yet. Its packages are in the **`testing`** suite for Ubuntu 26.04 (Resolute), and this page describes what that suite ships today. To get there from Ubuntu 25.10 or 25.04, follow [Upgrade to Ubuntu 26.04](/docs/howtos/upgrade-ubuntu-to-26-04/).
{{< /callout >}}

Each item says:
- what changed, and which session it applies to: **X11** ("Regolith / X11", i3) or **Wayland** ("Regolith / Wayland", sway);
- **Parity** when the feature worked in only one session in 3.4 and now works in both;
- the package that carries it, with its version in 3.5 today, and the pull request;
- how to try it, and what you should see.

## Before you begin

### Check that you have Regolith 3.5

In a terminal ({{< keys "super,enter" >}}):

```bash
cat /etc/regolith/version
apt policy regolith-desktop | head -4
```

You should see `REGOLITH_VERSION=3.5`, and `Installed: 4.11.0-1regolith-resolute` (or later) with the same `Candidate` version. If the candidate is newer, run `sudo apt update && sudo apt full-upgrade` first.

To see the versions of the packages that the items below refer to:

```bash
dpkg-query -W regolith-wm-config regolith-avizo regolith-powerd regolith-displayd regolith-inputd regolith-session-common ilia i3status-rs regolith-i3xrocks-config regolith-control-center sway-regolith regolith-look-default
```

### Switch sessions

Most items name a session. To change session:

1. Log out: press {{< keys "super,shift,e" >}}, then **Log Out** (X11) or **Logout** (Wayland).
1. On the login screen, click your name, then the gear icon at the bottom right.
1. Pick **Regolith / X11** or **Regolith / Wayland**, and type your password.

In a terminal, `echo $XDG_SESSION_TYPE` prints `x11` or `wayland`.

### What you need

Most checks need only a keyboard and a mouse. A few need hardware: a laptop for the brightness keys and the battery warning, a USB stick for automount, and a second monitor for the monitor labels. Some steps install a package or change a setting; each says how to undo it if it isn't obvious.

## Session parity at a glance

These features worked in only one session in Regolith 3.4. In 3.5 they work in both.

| Feature | 3.4 | 3.5 |
|---|---|---|
| [Volume indicator](#volume-indicator-on-every-key-press), [1.5 s timeout](#the-indicator-goes-away-after-15-seconds), [100% limit](#volume-keys-stop-at-100), [fine volume steps](#fine-volume-steps-and-modifier-variants) | X11 | both |
| [Brightness keys on the lock screen](#brightness-keys-on-the-lock-screen) | X11 | both |
| [Calculator and Home Folder keys](#calculator-and-home-folder-keys) | X11 | both |
| [Print screenshot keys](#print-screenshot-keys-in-wayland) | X11 | both |
| [Super+Print screenshot keys](#superprint-screenshot-keys-in-x11) | Wayland | both |
| [Screen Blank settings](#screen-blank-follows-settings), [screen off after locking](#the-screen-turns-off-soon-after-you-lock-it), [apps that lock the screen](#apps-and-loginctl-can-lock-the-screen) | X11 | both |
| [Logout and power off requests ask first](#logout-and-power-off-requests-ask-first) | X11 | both |
| [Automount](#usb-sticks-mount-automatically), [low-battery warning](#low-battery-warning-and-automatic-power-saver) | X11 | both |
| ["Keep these display settings?"](#keep-these-display-settings), [monitor labels](#monitor-numbers-on-screen) | X11 | both |
| [Compose key](#compose-key-and-other-keyboard-options), [key repeat](#key-repeat-settings), [system keyboard layout on first login](#a-new-account-gets-the-system-keyboard-layout) | X11 | both |
| [Background from Settings](#settings-sets-the-wayland-wallpaper), [theme of X11 apps](#x11-apps-follow-the-theme-in-wayland), [look wallpaper colours](#look-wallpaper-colours-and-gradients-in-wayland) | X11 | both |
| [CPU block](#the-wayland-bar-shows-the-same-blocks), [12/24-hour clock](#the-wayland-clock-follows-the-time-format) on the bar | X11 | both |
| [Gap keys](#gap-keys-in-wayland), [Terminate App](#terminate-app-in-wayland) | X11 | both |
| [Next Free Workspace keys](#next-free-workspace-keys-in-x11), [dialogs in the window switcher](#dialogs-in-the-window-switcher-on-x11) | Wayland | both |

## Volume, brightness and media keys

### Volume indicator on every key press

**Wayland** · **Parity** · `regolith-avizo` 0.1.7 · [avizo#6](https://github.com/regolith-linux/avizo/pull/6)

On Ubuntu 26.04 the volume keys changed the volume without showing the on-screen indicator, unless the sound was muted.

1. Press {{< keys "volume up" >}}, then {{< keys "volume down" >}}.

**You should see:** a box in the middle of the screen with a speaker icon and a level bar, on every press. The volume block on the bar follows.

### The indicator goes away after 1.5 seconds

**Wayland** · **Parity** · `regolith-avizo` 0.1.7 · [avizo#8](https://github.com/regolith-linux/avizo/pull/8), [Discussion #1107](https://github.com/orgs/regolith-linux/discussions/1107)

The indicator used to stay on screen for about 5 seconds. It now hides after 1.5 seconds, as in the X11 session.

1. Press {{< keys "volume up" >}} once and count.

**You should see:** the indicator disappear between one and two seconds after the key press. You can set another time with `time = ` in `~/.config/avizo/config.ini`.

### Volume keys stop at 100%

**Wayland** · **Parity** · `regolith-avizo` 0.1.7 · [avizo#9](https://github.com/regolith-linux/avizo/pull/9)

The volume keys used to raise the volume past 100% without limit.

1. Set the volume close to the top: `pactl set-sink-volume @DEFAULT_SINK@ 97%`
1. Press {{< keys "volume up" >}} twice.
1. Run `pactl get-sink-volume @DEFAULT_SINK@`.

**You should see:** `100%`, not 105% or more.

### Fine volume steps and modifier variants

**Wayland** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#98](https://github.com/regolith-linux/regolith-wm-config/pull/98)

The modifier variants of the volume and player keys that the X11 session handles now work in sway too.

1. Run `pactl set-sink-volume @DEFAULT_SINK@ 50%`.
1. Press {{< keys "shift,volume up" >}}, then {{< keys "ctrl,volume up" >}}, then {{< keys "alt,volume up" >}}, running `pactl get-sink-volume @DEFAULT_SINK@` after each.
1. Press {{< keys "alt,mute" >}} twice.

**You should see:** 52% (Shift makes a fine 2% step), then 57% and 62% (Ctrl and Alt act like the plain key). {{< keys "alt,mute" >}} mutes and unmutes.

### Brightness keys on the lock screen

**Wayland** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#74](https://github.com/regolith-linux/regolith-wm-config/pull/74), fixes [regolith-wm-config#73](https://github.com/regolith-linux/regolith-wm-config/issues/73)

On a laptop, the brightness keys did nothing while the screen was locked, so a screen dimmed all the way stayed dark.

1. Press {{< keys "brightness up" >}}: the indicator shows a brightness icon and the level.
1. Lock the screen with {{< keys "super,escape" >}}.
1. Press {{< keys "brightness down" >}} and {{< keys "brightness up" >}}.

**You should see:** the brightness change while the lock screen is up.

### Sharp indicator on scaled displays

**Wayland** · `regolith-avizo` 0.1.7 · [avizo#7](https://github.com/regolith-linux/avizo/pull/7)

On a display with a scale above 100%, the indicator's icons were blurry.

1. Run `swaymsg output '*' scale 2` (on a HiDPI display your scale may already be above 1).
1. Press {{< keys "volume down" >}}.
1. Run `swaymsg output '*' scale 1` to go back.

**You should see:** a crisp speaker icon with sharp edges.

### Calculator and Home Folder keys

**Wayland** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#97](https://github.com/regolith-linux/regolith-wm-config/pull/97)

Keyboards with launcher keys can use them in sway as in the X11 session.

1. Press the Calculator key.
1. Press the Home Folder (Explorer) key.

**You should see:** GNOME Calculator (if it is installed, also as a snap), then Files.

## Screenshots

### Print screenshot keys in Wayland

**Wayland** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#78](https://github.com/regolith-linux/regolith-wm-config/pull/78)

In 3.4, plain {{< keys "print" >}} did nothing in sway. It now has the same keys as the X11 session.

| Key | Result |
|---|---|
| {{< keys "print" >}} | Saves the whole screen to `~/Pictures` |
| {{< keys "shift,print" >}} | Drag an area with the mouse; saves it to `~/Pictures` |
| {{< keys "alt,print" >}} | Saves the focused window to `~/Pictures` |
| {{< keys "ctrl,print" >}}, {{< keys "ctrl,shift,print" >}}, {{< keys "ctrl,alt,print" >}} | The same, copied to the clipboard instead |

1. Press {{< keys "print" >}} and {{< keys "alt,print" >}}, and run `ls -t ~/Pictures | head -2`.
1. Press {{< keys "ctrl,print" >}} and run `wl-paste -l`.

**You should see:** two new PNG files, then `image/png` in the clipboard. Each screenshot adds a notification (the bell count on the bar goes up; {{< keys "super,n" >}} lists them). The older {{< keys "super,print" >}} keys still work.

### Super+Print screenshot keys in X11

**X11** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#96](https://github.com/regolith-linux/regolith-wm-config/pull/96)

The three screenshot keys that only the Wayland session had are now in the X11 session too.

| Key | Result |
|---|---|
| {{< keys "super,print" >}} | Copies the focused window to the clipboard |
| {{< keys "super,shift,print" >}} | Click a window or drag an area; saves it to `~/Pictures` and copies it |
| {{< keys "super,shift,ctrl,print" >}} | Saves all screens to `~/Pictures` |

1. Press {{< keys "super,print" >}} and run `xclip -selection clipboard -t TARGETS -o | grep png`.
1. Press {{< keys "super,shift,print" >}}, click a window, and run `ls -t ~/Pictures | head -1`.

**You should see:** `image/png`, then a new `Screenshot from …png` with the size of that window, and a "Screenshot of anything" notification.

## Lock screen, idle and power

### Screen Blank follows Settings

**Wayland** · **Parity** · `regolith-powerd` 0.10.0 · [regolith-powerd#24](https://github.com/regolith-linux/regolith-powerd/pull/24), [regolith-powerd#44](https://github.com/regolith-linux/regolith-powerd/pull/44)

sway now follows Settings › Power as the X11 session does. In 3.4, turning screen blanking off still blanked the screen after 5 seconds and locked it after 10, and the dim, blank and lock times didn't match the settings.

1. Open Settings ({{< keys "super,c" >}}) › **Power**.
1. Turn **Automatic Screen Blank** off, and leave the mouse and keyboard alone for two minutes.
1. Turn it back on, set **Delay** to **1 minute**, and leave the machine alone for a little over a minute.
1. Set **Delay** back to your usual value.

**You should see:** with blanking off, the screen stays on and unlocked (it may dim). With 1 minute, the screen turns off and the session locks after one minute.

### The screen turns off soon after you lock it

**Wayland** · **Parity** · `regolith-powerd` 0.10.0 · [regolith-powerd#39](https://github.com/regolith-linux/regolith-powerd/pull/39)

After {{< keys "super,escape" >}} the lock screen used to stay lit until the normal idle timeout (more than 5 minutes).

1. Lock with {{< keys "super,escape" >}}, and don't touch anything.
1. Lock again, press a key to wake the lock screen, and wait.

**You should see:** the screen turn off about 5 seconds after locking, and 30 seconds after the last key press on the lock screen.

### Apps and loginctl can lock the screen

**Wayland** · **Parity** · `regolith-powerd` 0.10.0 · [regolith-powerd#25](https://github.com/regolith-linux/regolith-powerd/pull/25)

sway now provides GNOME's screen saver interface. Before, `xdg-screensaver lock` did nothing, and password managers that lock with the screen (KeePassXC, for example) stayed unlocked.

1. Run `loginctl lock-session`, and unlock with your password.
1. Run `xdg-screensaver lock`, and unlock.

**You should see:** the lock screen each time.

### Switch keyboard layout on the lock screen

**Wayland** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#63](https://github.com/regolith-linux/regolith-wm-config/pull/63)

With more than one input source (Settings › Keyboard › **Add Input Source**), you can now change the layout while the screen is locked, for example to type a password in another layout.

1. Add a second input source, such as German.
1. Lock with {{< keys "super,escape" >}} and press {{< keys "super,alt,backspace" >}}.
1. Unlock, and run `swaymsg -t get_inputs | grep -m1 xkb_active_layout_name`.

**You should see:** the second layout (here `"German"`). Press {{< keys "super,alt,backspace" >}} again to go back.

### Logout and power off requests ask first

**Wayland** · **Parity** · `regolith-powerd` 0.10.0 · [regolith-powerd#27](https://github.com/regolith-linux/regolith-powerd/pull/27)

Logout, restart and power off requests from applications or the command line used to do nothing in sway.

1. Run `gnome-session-quit --logout` in a terminal.

**You should see:** a yellow bar at the top: "Do you really want to logout? You will be logged out automatically in 60 seconds." Click **X** to cancel. **Logout**, or waiting 60 seconds, logs you out.

### USB sticks mount automatically

**Wayland** · **Parity** · `regolith-powerd` 0.10.0 · [regolith-powerd#30](https://github.com/regolith-linux/regolith-powerd/pull/30)

Plug in a USB stick or an SD card.

**You should see:** it is mounted under `/run/media/$USER/` and opens in Files, as in the X11 session (`findmnt /run/media/$USER/*` lists it). This follows GNOME's `org.gnome.desktop.media-handling` settings.

### Low-battery warning and automatic Power Saver

**Wayland** · **Parity** · `regolith-powerd` 0.10.0 · [regolith-powerd#23](https://github.com/regolith-linux/regolith-powerd/pull/23), fixes [regolith-desktop#355](https://github.com/regolith-linux/regolith-desktop/issues/355); [regolith-powerd#38](https://github.com/regolith-linux/regolith-powerd/pull/38)

On a laptop, unplug the charger and let the battery run down to the low level (about 10%).

**You should see:** a "Power — Low Battery — 9% battery remaining" notification (with your percentage) ({{< keys "super,n" >}}), and `powerprofilesctl get` switch from `balanced` to `power-saver`. When you plug the charger back in, the profile goes back to `balanced`.

### No more crash reports from the audio idle inhibitor on X11

**X11** · `sway-audio-idle-inhibit` 2.0.4 · [SwayAudioIdleInhibit#9](https://github.com/regolith-linux/SwayAudioIdleInhibit/pull/9)

The Wayland session's audio idle inhibitor also started in the X11 session, where it crashed and caused "System program problem detected" dialogs.

1. In the X11 session, run `systemctl --user is-active sway-audio-idle-inhibit`.
1. In the Wayland session, run it again.

**You should see:** `inactive` on X11 and `active` on Wayland.

## Displays

### Keep these display settings?

**Wayland** · **Parity** · `regolith-displayd` 0.5.0 · [regolith-displayd#28](https://github.com/regolith-linux/regolith-displayd/pull/28), fixes [regolith-displayd#15](https://github.com/regolith-linux/regolith-displayd/issues/15)

A display change from Settings now asks for confirmation, and goes back by itself if you don't confirm, so a wrong mode can't leave you with a black screen.

1. Open Settings ({{< keys "super,c" >}}) › **Displays**.
1. Change **Scale** (for example to 125%), and click **Apply**.
1. Don't press anything for 20 seconds.

**You should see:** "Keep these display settings? Settings changes will revert in … seconds", counting down from 20, with "Enter: Keep Changes" and "Esc: Revert Settings". After 20 seconds the old scale comes back. Press {{< keys "enter" >}} instead to keep the change.

### Monitor numbers on screen

**Wayland** · **Parity** · `regolith-displayd` 0.5.0, `regolith-powerd` 0.10.0 · [regolith-displayd#18](https://github.com/regolith-linux/regolith-displayd/pull/18), [regolith-powerd#28](https://github.com/regolith-linux/regolith-powerd/pull/28), fixes [regolith-displayd#16](https://github.com/regolith-linux/regolith-displayd/issues/16); [regolith-displayd#22](https://github.com/regolith-linux/regolith-displayd/pull/22)

With two or more monitors, open Settings ({{< keys "super,c" >}}) › **Displays**.

**You should see:** a number in the top left corner of each monitor, matching the numbers in the panel. The panel names each monitor by maker and size, as in the X11 session.

## Keyboard and input

### Compose key and other keyboard options

**Wayland** · **Parity** · `regolith-inputd` 0.5.0 · [regolith-inputd#12](https://github.com/regolith-linux/regolith-inputd/pull/12), fixes [regolith-inputd#6](https://github.com/regolith-linux/regolith-inputd/issues/6)

1. Open Settings ({{< keys "super,c" >}}) › **Keyboard** › **Compose Key**, turn it on and choose **Right Alt**.
1. In a terminal, press and release {{< keys "right alt" >}}, then type `o` `c`; then {{< keys "right alt" >}}, `'`, `e`.

**You should see:** `©` and `é`. In 3.4 the setting had no effect in sway.

### Key repeat settings

**Wayland** · **Parity** · `regolith-inputd` 0.5.0 · [regolith-inputd#14](https://github.com/regolith-linux/regolith-inputd/pull/14)

The repeat settings now apply when you log in, and turning repeat off works.

1. Open Settings › **Accessibility** › **Typing**, turn **Repeat Keys** off, and run `swaymsg -t get_inputs | grep -m1 repeat_rate`: it prints `"repeat_rate": 0`. Turn it back on.
1. Run `gsettings set org.gnome.desktop.peripherals.keyboard delay 250`, log out and in again, and run `swaymsg -t get_inputs | grep -m1 repeat_delay`.
1. Run `gsettings reset org.gnome.desktop.peripherals.keyboard delay` to go back.

**You should see:** `"repeat_delay": 250` after the new login.

### A new account gets the system keyboard layout

**Wayland** · **Parity** · `regolith-session` 1.2.5 · [regolith-session#78](https://github.com/regolith-linux/regolith-session/pull/78)

A user whose first login was "Regolith / Wayland" got the US layout, whatever the system keyboard was set to.

1. On a system whose keyboard layout isn't US (`localectl status` shows it), create a user, and log in to **Regolith / Wayland** for the first time.

**You should see:** the system layout on the bar's keyboard block (for example "French").

### Sticky keys, slow keys, bounce keys and mouse keys in sway

**Wayland** · `sway-regolith` 1.11.2 · [sway-regolith#48](https://github.com/regolith-linux/sway-regolith/pull/48)

sway has new keyboard accessibility options. The toggles in Settings › Accessibility › Typing don't reach sway yet, so set them with `swaymsg` or in your sway configuration.

1. Run `swaymsg input type:keyboard sticky_keys enabled`.
1. Press and release {{< keys "shift" >}}, then press `a` in a terminal.
1. Run `swaymsg input type:keyboard sticky_keys disabled`.

**You should see:** `A`. `swaymsg -t get_inputs | grep -m1 sticky_keys` shows the state; `man sway-input` lists `slow_keys`, `bounce_keys` and `mouse_keys`.

## Bar

### The Wayland bar shows the same blocks

**Wayland** · **Parity** · `i3status-rs` 0.40.0-2 · [i3status-rs_debian#25](https://github.com/regolith-linux/i3status-rs_debian/pull/25)

The sway bar has a CPU block, as the X11 bar has, and no longer shows an empty red battery on machines without a battery.

**You should see:** a CPU percentage between the network and disk blocks, and a battery block only on a laptop.

### The Wayland clock follows the time format

**Wayland** · **Parity** · `i3status-rs` 0.40.0-2 · [i3status-rs_debian#26](https://github.com/regolith-linux/i3status-rs_debian/pull/26)

1. In the evening, run `gsettings set org.gnome.desktop.interface clock-format 12h` (the Time Format setting of Settings › System › Date & Time).
1. Wait 5 seconds, then run `gsettings set org.gnome.desktop.interface clock-format 24h`.

**You should see:** the bar's clock change from 24-hour time (`20:05`) to 12-hour time (`08:05`) and back, without restarting anything.

### AM and PM on the X11 clock

**X11** · `regolith-i3xrocks-config` 6.0.0 · [regolith-i3xrocks-config#170](https://github.com/regolith-linux/regolith-i3xrocks-config/pull/170)

1. Run `gsettings set org.gnome.desktop.interface clock-format 12h`, then press {{< keys "super,shift,r" >}}.

**You should see:** the time with `AM` or `PM`, for example `09:04 PM`.

### Volume and microphone blocks on PipeWire

**X11** · **Parity** · `regolith-i3xrocks-config` 6.0.0 · [regolith-i3xrocks-config#161](https://github.com/regolith-linux/regolith-i3xrocks-config/pull/161), [regolith-i3xrocks-config#167](https://github.com/regolith-linux/regolith-i3xrocks-config/pull/167), fixes [regolith-desktop#1087](https://github.com/regolith-linux/regolith-desktop/issues/1087); [regolith-i3xrocks-config#175](https://github.com/regolith-linux/regolith-i3xrocks-config/pull/175)

The X11 volume and microphone blocks showed no level on PipeWire, the default on Ubuntu.

1. Install the blocks if you don't have them: `sudo apt install i3xrocks-volume i3xrocks-microphone`, then press {{< keys "super,shift,r" >}}.
1. Scroll up over the volume block until it stops.

**You should see:** a speaker and a microphone with their levels. Scrolling stops at 100%.

### The CPU block turns red under load and opens System Monitor

**X11** · `regolith-i3xrocks-config` 6.0.0 · [regolith-i3xrocks-config#168](https://github.com/regolith-linux/regolith-i3xrocks-config/pull/168), [regolith-i3xrocks-config#171](https://github.com/regolith-linux/regolith-i3xrocks-config/pull/171)

1. Load every CPU for 15 seconds: `for i in $(seq $(nproc)); do timeout 15 sh -c 'while :; do :; done' & done`
1. Click the CPU block.

**You should see:** the CPU value in red while the load runs (it turns to the warning colour from 50% and to red from 80%). The click opens System Monitor in a floating window.

## Launcher (ilia)

All of these are in `ilia` 0.19.2 and work in both sessions. Open the launcher with {{< keys "super,space" >}}.

### Search with several words, in any order

**X11 and Wayland** · [ilia#128](https://github.com/regolith-linux/ilia/pull/128), fixes [ilia#77](https://github.com/regolith-linux/ilia/issues/77); [ilia#151](https://github.com/regolith-linux/ilia/pull/151)

1. Type `monitor system`.
1. Clear it and type `editor text`.

**You should see:** System Monitor, then Vim (found by its generic name "Text Editor"). Each word must match, in any order, in the name, generic name or keywords.

### Home and End

**X11 and Wayland** · [ilia#129](https://github.com/regolith-linux/ilia/pull/129), fixes [ilia#36](https://github.com/regolith-linux/ilia/issues/36)

Press {{< keys "end" >}}, then {{< keys "home" >}}.

**You should see:** the last row selected, then the first.

### Run a command that isn't in the list

**X11 and Wayland** · [ilia#127](https://github.com/regolith-linux/ilia/pull/127), fixes [ilia#3](https://github.com/regolith-linux/ilia/issues/3)

1. Open the command launcher with {{< keys "super,shift,space" >}}.
1. Type `touch /tmp/ilia-test` and press {{< keys "enter" >}}.
1. Run `ls /tmp/ilia-test`.

**You should see:** the file exists. In 3.4, Enter did nothing when the text matched no item.

### Terminal applications open in a terminal

**X11 and Wayland** · [ilia#131](https://github.com/regolith-linux/ilia/pull/131), fixes [ilia#92](https://github.com/regolith-linux/ilia/issues/92)

Type `htop` and press {{< keys "enter" >}}.

**You should see:** the program running in a terminal window.

### Newest notification first, and Delete removes one

**X11 and Wayland** · [ilia#132](https://github.com/regolith-linux/ilia/pull/132), fixes [regolith-desktop#607](https://github.com/regolith-linux/regolith-desktop/issues/607); [ilia#137](https://github.com/regolith-linux/ilia/pull/137)

1. Run `notify-send first-note; sleep 1; notify-send second-note`.
1. Open the notifications with {{< keys "super,n" >}}, and press {{< keys "delete" >}}.

**You should see:** second-note at the top and highlighted; after Delete only first-note is left, highlighted.

### Right Super closes, keypad Enter launches

**X11 and Wayland** · [ilia#155](https://github.com/regolith-linux/ilia/pull/155), [ilia#136](https://github.com/regolith-linux/ilia/pull/136)

1. Open the launcher and press the right {{< keys "super" >}} key.
1. Open it again, type `calc`, and press {{< keys "enter" >}} on the numeric keypad.

**You should see:** the launcher closes, as with the left Super key; then Calculator starts.

### Run a key binding from the keybinding viewer

**X11 and Wayland** · [ilia#158](https://github.com/regolith-linux/ilia/pull/158), [ilia#159](https://github.com/regolith-linux/ilia/pull/159), [ilia#161](https://github.com/regolith-linux/ilia/pull/161)

1. Open the keybinding viewer with {{< keys "super,shift,?" >}}.
1. Type `terminal`, and press {{< keys "enter" >}} on "Launch - Terminal".

**You should see:** a new terminal window.

### Dialogs in the window switcher on X11

**X11** · **Parity** · [ilia#147](https://github.com/regolith-linux/ilia/pull/147)

1. Run `zenity --question --title=test-dialog --text=hello &`.
1. Open the window switcher with {{< keys "super,ctrl,space" >}}.

**You should see:** test-dialog in the list, as in the Wayland session. In 3.4 the X11 list left dialog windows out.

## Windows and workspaces

### Switch back to the previous workspace

**X11 and Wayland** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#80](https://github.com/regolith-linux/regolith-wm-config/pull/80), fixes [regolith-desktop#591](https://github.com/regolith-linux/regolith-desktop/issues/591)

1. On workspace 1, press {{< keys "super,3" >}}.
1. Press {{< keys "super,alt,tab" >}} twice.

**You should see:** workspace 1, then 3 again. The key is `wm.binding.ws_back_and_forth`.

### Resize mode arrows go the right way

**X11 and Wayland** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#87](https://github.com/regolith-linux/regolith-wm-config/pull/87)

1. Open two terminals one above the other: {{< keys "super,enter" >}}, {{< keys "super,v" >}}, {{< keys "super,enter" >}}.
1. Press {{< keys "super,r" >}}, then {{< keys "up" >}} a few times, then {{< keys "escape" >}}.

**You should see:** the focused window grow taller with {{< keys "up" >}} and shorter with {{< keys "down" >}}, like {{< keys "k" >}} and {{< keys "j" >}}. In 3.4 the arrows did the opposite.

### Gap keys in Wayland

**Wayland** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#92](https://github.com/regolith-linux/regolith-wm-config/pull/92)

1. Open two windows side by side.
1. Press {{< keys "super,=" >}}, then {{< keys "super,shift,=" >}}, then {{< keys "super,minus" >}} and {{< keys "super,shift,minus" >}}.

**You should see:** the gaps between the windows grow, grow more, and shrink back.

### Terminate App in Wayland

**Wayland** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#70](https://github.com/regolith-linux/regolith-wm-config/pull/70)

Focus a terminal (foot, a native Wayland app) and press {{< keys "super,alt,q" >}}.

**You should see:** the window is killed. In 3.4 this key only worked on X11 apps.

### Next Free Workspace keys in X11

**X11** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#71](https://github.com/regolith-linux/regolith-wm-config/pull/71)

With a window open on workspace 1, press {{< keys "super,`" >}}.

**You should see:** the first empty workspace. {{< keys "super,shift,`" >}} moves the focused window to it, and {{< keys "super,alt,`" >}} takes the window and follows it.

### Swapping Super and Alt works

**X11 and Wayland** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#63](https://github.com/regolith-linux/regolith-wm-config/pull/63), fixes [regolith-desktop#504](https://github.com/regolith-linux/regolith-desktop/issues/504), [regolith-desktop#1098](https://github.com/regolith-linux/regolith-desktop/issues/1098)

1. Add `wm.mod: Mod1` and `wm.alt: Mod4` to `~/.config/regolith3/Xresources`, and press {{< keys "super,shift,r" >}}.
1. Press {{< keys "alt,3" >}}, then {{< keys "alt,1" >}}.
1. Remove the two lines and press {{< keys "alt,shift,r" >}}.

**You should see:** no configuration error bar, and Alt switching workspaces.

## Looks and appearance

### Dark looks reach GTK 4 apps

**X11 and Wayland** · `regolith-look-default` 0.9.0, `regolith-look-extra` 0.11.0 · [regolith-look-default#9](https://github.com/regolith-linux/regolith-look-default/pull/9), [regolith-look-default#10](https://github.com/regolith-linux/regolith-look-default/pull/10), [regolith-look-extra#25](https://github.com/regolith-linux/regolith-look-extra/pull/25), fixes [regolith-look-extra#19](https://github.com/regolith-linux/regolith-look-extra/issues/19)

Every look now sets GNOME's light or dark style, so GTK 4 and libadwaita apps follow it.

1. Run `gsettings get org.gnome.desktop.interface color-scheme`.
1. Open Settings ({{< keys "super,c" >}}) or Calculator.

**You should see:** `'prefer-dark'` with the default look, and dark windows. Settings › **Appearance** shows **Dark** selected.

### Settings sets the Wayland wallpaper

**Wayland** · **Parity** · `regolith-control-center` 1.50.5 · [regolith-control-center#31](https://github.com/regolith-linux/regolith-control-center/pull/31)

1. Open Settings ({{< keys "super,c" >}}) › **Appearance**, scroll to **Background**, and click a picture.
1. Lock the screen with {{< keys "super,escape" >}}.

**You should see:** the picture as the desktop background right away, and on the lock screen. It is saved in `~/.config/regolith3/Xresources` (`regolith.wallpaper.file`); remove those lines and press {{< keys "super,shift,r" >}} to go back to the look's wallpaper.

### X11 apps follow the theme in Wayland

**Wayland** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#69](https://github.com/regolith-linux/regolith-wm-config/pull/69)

Apps that run under XWayland used to get the default light theme, fonts and cursor.

1. Run `GDK_BACKEND=x11 nm-connection-editor`.

**You should see:** the window in the same theme and font as native apps (dark with the default look).

### Look wallpaper colours and gradients in Wayland

**Wayland** · **Parity** · `regolith-wm-config` 4.12.0 · [regolith-wm-config#82](https://github.com/regolith-linux/regolith-wm-config/pull/82)

Looks without a wallpaper picture define colours (`regolith.wallpaper.color.primary`, `…secondary`, `…shading.type`). sway showed black for colour names and drew no gradients.

1. With the default look and no wallpaper picture set, look at the desktop.

**You should see:** a vertical grey gradient, as in the X11 session.

### Looks in their own style everywhere

**X11 and Wayland** · `regolith-wm-config` 4.12.0, `regolith-ftue` 2.2.3 · [regolith-wm-config#88](https://github.com/regolith-linux/regolith-wm-config/pull/88), [regolith-ftue#10](https://github.com/regolith-linux/regolith-ftue/pull/10)

1. Open the Look Selector with {{< keys "super,alt,l" >}}.

**You should see:** the selector in the colours of your look, like the launcher. The keybinding help shown on your first login uses the look's style too.

### Updated Dracula, Nord and Nevil themes

**X11 and Wayland** · `dracula-gtk` 4.0.0, `nordic` 2.2.0, `whitesur-gtk-theme` 1.2 · [dracula-gtk#3](https://github.com/regolith-linux/dracula-gtk/pull/3), [nordic#3](https://github.com/regolith-linux/nordic/pull/3), [WhiteSur-gtk-theme#4](https://github.com/regolith-linux/WhiteSur-gtk-theme/pull/4)

The GTK themes of the Dracula, Nord and Nevil looks are new upstream releases.

1. Run `sudo apt install regolith-look-dracula`.
1. Pick **dracula** in the Look Selector ({{< keys "super,alt,l" >}}), and run `gsettings get org.gnome.desktop.interface gtk-theme`.

**You should see:** `'Dracula'`, with the Dracula icons. Pick your usual look again afterwards.

### Workspace names with accents and other non-ASCII text

**X11 and Wayland** · `regolith-session` 1.2.5 · [regolith-session#85](https://github.com/regolith-linux/regolith-session/pull/85)

1. Add `wm.workspace.02.name: 2:Ñandú café` to `~/.config/regolith3/Xresources`.
1. Log out and in again, and press {{< keys "super,2" >}}.

**You should see:** "Ñandú café" on the workspace button, not escape codes.

### Removing an override takes effect without logging out

**X11 and Wayland** · `regolith-session` 1.2.5 · [regolith-session#98](https://github.com/regolith-linux/regolith-session/pull/98)

1. Add `wm.bar.position: top` to `~/.config/regolith3/Xresources` and press {{< keys "super,shift,r" >}}.
1. Remove the line and press {{< keys "super,shift,r" >}} again.

**You should see:** the bar move to the top, then back to the bottom. In 3.4 a removed value stayed until the next login.

## Settings app

### The shortcut list matches the session

**X11 and Wayland** · `regolith-control-center` 1.50.5 · [regolith-control-center#30](https://github.com/regolith-linux/regolith-control-center/pull/30)

Open Settings ({{< keys "super,c" >}}) › **Keyboard** › **View and Customize Shortcuts**.

**You should see:**
- X11: a **Screenshots** section with the keys the session really uses ({{< keys "print" >}} saves a screenshot, and so on), so you can change them.
- Wayland: no Navigation, Windows or Screenshots sections, because sway handles those keys.

## Sessions and first login

### No GNOME welcome wizard

**X11 and Wayland** · `regolith-session` 1.2.5 · [regolith-session#70](https://github.com/regolith-linux/regolith-session/pull/70)

Create a user and log in to a Regolith session for the first time.

**You should see:** the Regolith keybinding help, and no GNOME "Welcome" window.

### The X11 compositor works without 3D acceleration

**X11** · `regolith-compositor-picom-glx` 1.4.1 · [regolith-compositor-picom-glx#17](https://github.com/regolith-linux/regolith-compositor-picom-glx/pull/17), [regolith-compositor-picom-glx#16](https://github.com/regolith-linux/regolith-compositor-picom-glx/pull/16), fixes [regolith-compositor-picom-glx#4](https://github.com/regolith-linux/regolith-compositor-picom-glx/issues/4)

In virtual machines and on machines that render in software, picom now uses its xrender backend, and it starts again if it crashes.

1. Run `pgrep -a picom`.
1. Run `kill -9 $(pgrep -x picom)`, wait a few seconds, and run `pgrep -a picom` again.

**You should see:** picom running (with `--backend xrender` on a software renderer), and running again with a new process ID.
