---
title: "Upgrade to Ubuntu 26.04"
linkTitle: "Upgrade to Ubuntu 26.04"
aliases:
  - /docs/howtos/upgrade-ubuntu-25-10-to-26-04/
description: >
  Upgrade an Ubuntu 25.10 or 25.04 system running Regolith to Ubuntu 26.04 LTS
---

This page takes an Ubuntu 25.10 (Questing) or Ubuntu 25.04 (Plucky) system with Regolith, installed as described on the [install page](/docs/using-regolith/install/), through Ubuntu's `do-release-upgrade` to Ubuntu 26.04 LTS (Resolute), and then brings Regolith back.

The Ubuntu upgrader disables the Regolith package repository and removes a few Regolith packages, so some extra steps are needed before and after it runs. Do them in the order below.

{{< callout type="warning" >}}
Regolith has no stable release for Ubuntu 26.04 yet: `https://archive.regolith-desktop.com/ubuntu/stable` has no `resolute` suite. Until Regolith 3.5 is released, this page uses the **`testing`** suite. It already carries the Regolith 3.5 packages, built from the same release tags that the stable release will use. [Move to the stable suite](#move-to-the-stable-release-later) once the install page lists it for Ubuntu 26.04.
{{< /callout >}}

## Which release are you on?

| You have | Regolith | What is different |
|---|---|---|
| Ubuntu 25.10 | 3.4 | Nothing: follow the steps as written. |
| Ubuntu 25.04 | 3.3 or 3.4 | You upgrade **directly to 26.04**, without stopping at 25.10. Ubuntu 25.10 has reached its end of life too, so the upgrader skips it. Don't use `do-release-upgrade -d`: it would pick 25.10. |

The steps are the same for both releases. Where a command or its output differs, the step says so, or shows a tab for each release.

## Overview

1. Install all updates for your current release, pin your look, and remove one stale systemd link.
1. Run `sudo do-release-upgrade` from a text console.
1. When the upgrader asks to restart, answer **N**.
1. On the same console, point Regolith at `testing resolute` and upgrade the Regolith packages.
1. Reboot, log in to both Regolith sessions, and clean up.

The download is about 2.5 GB, and the upgrade itself takes 45 minutes to an hour on a fast machine with a fast connection. On a laptop, plan 1.5 to 3 hours, and keep it on AC power.

## Before you start

### Check where you are

```bash
lsb_release -ds
cat /etc/regolith/version
cat /etc/apt/sources.list.d/regolith.list
ls /etc/apt/sources.list.d/
df -h / /boot
```

A system installed from the install page has the key in `/usr/share/keyrings/regolith-archive-keyring.gpg` and one of these lines in `/etc/apt/sources.list.d/regolith.list`:

| Release | Line in `regolith.list` |
|---|---|
| Ubuntu 25.10, Regolith 3.4 | `deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg] https://archive.regolith-desktop.com/ubuntu/stable questing v3.4` |
| Ubuntu 25.04, Regolith 3.3 | `deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg] https://archive.regolith-desktop.com/ubuntu/stable plucky v3.3` |
| Ubuntu 25.04, Regolith 3.4 | the same line with `v3.4` instead of `v3.3` |

The key stays valid for Ubuntu 26.04 and is reused below. Write down your line: you need it again only if the upgrader aborts ([Recovery](#recovery)).

If you already use the `testing` or `unstable` suite, the same steps work. Your source file may be called `regolith.sources` instead of `regolith.list`; step 5 replaces either one. If you added the Regolith repository under any other file name, delete that file in step 5, so that the repository isn't listed twice.

### Checklist

| Check | Command | What to do |
|---|---|---|
| Free disk space | `df -h / /boot` (above) | Have at least 10 GB free on `/` and 300 MB on `/boot`. |
| Third-party repositories and PPAs | `ls /etc/apt/sources.list.d/` (above) | The upgrader disables all of them. Write them down, so you can re-enable each one once it publishes packages for `resolute`. |
| Proprietary graphics drivers | `ubuntu-drivers list` | The upgrader keeps your `nvidia-driver-*` series and rebuilds its kernel modules. Have a second way in (text console or ssh) in case the screen stays black; see [Recovery](#recovery). |
| Secure Boot with DKMS modules | `mokutil --sb-state` | You may be asked to enroll a key (MOK) during the upgrade. Choose a one-time password and confirm it on the blue MOK screen at the next boot. |
| Snaps and Flatpaks | `snap list`, `flatpak list` | Snaps are kept and moved to their 26.04 channels. Flatpaks are untouched; run `flatpak update` afterwards. |
| Your Regolith configuration | `ls ~/.config/regolith3/` | Kept unchanged. Back it up anyway (next section). |

### Back up

Make a full backup of your home directory with your usual tool. Then save the Regolith configuration, the apt configuration and the list of installed Regolith packages:

```bash
mkdir -p ~/upgrade-backup
tar czf ~/upgrade-backup/regolith-user-config.tgz -C ~ .config/regolith3
sudo tar czf ~/upgrade-backup/etc-regolith-and-apt.tgz /etc/regolith /etc/apt
dpkg-query -W -f='${db:Status-Abbrev} ${Package} ${Version}\n' | awk '$1=="ii"{print $2" "$3}' | grep -E 'regolith|^i3|^sway|ilia|trawl|xrescat|picom|gtklock|rofication|remontoire|avizo|dunst' > ~/upgrade-backup/regolith-packages-before.txt
apt-mark showmanual > ~/upgrade-backup/manual-packages-before.txt
wc -l ~/upgrade-backup/regolith-packages-before.txt
```

It prints two `tar: Removing leading '/'` notices and the number of Regolith-related packages. Step 6 uses this list to find anything that the upgrade removed and didn't bring back.

## Steps

### 1. Install all updates for your current release

```bash
sudo apt update
sudo apt full-upgrade
[ -f /var/run/reboot-required ] && echo "reboot first" || echo "no reboot needed"
```

Reboot if it says so. `do-release-upgrade` refuses to start while updates are pending.

Ubuntu 25.10 and 25.04 have both reached their end of life:
- Ignore the "Software updates are no longer provided" dialog, and don't use its upgrade button. Use the console steps below instead.
- If `apt update` fails with `404 Not Found` errors for `questing` or `plucky`, the archive of your release has moved to `old-releases.ubuntu.com`. Point apt there, then continue (the upgrader switches back to the normal mirror):

  ```bash
  sudo sed -i 's#http://\(archive\|security\).ubuntu.com#http://old-releases.ubuntu.com#' /etc/apt/sources.list.d/ubuntu.sources
  sudo apt update
  ```

Then check that Ubuntu 26.04 is offered:

```bash
grep '^Prompt' /etc/update-manager/release-upgrades
do-release-upgrade -c
```

It should print `Prompt=normal`, "Your Ubuntu release is not supported anymore", and `New release '26.04.1 LTS' available.` (or a later point release).
- If it says `Prompt=never`, run `sudo sed -i 's/^Prompt=.*/Prompt=normal/' /etc/update-manager/release-upgrades` and check again.
- Don't add `-d`. On 25.04, `-d` selects 25.10, which no longer receives updates.

### 2. Pin your look (recommended)

If you never chose a look with `regolith-look set`, a later package update can switch you to another one. Pin the current look. Run this in a terminal inside a Regolith session, on X11 or Wayland:

```bash
grep -s regolith.look.path ~/.config/regolith3/Xresources || regolith-look set "$(if [ "$XDG_SESSION_TYPE" = wayland ]; then trawlcat regolith.look; else xrescat regolith.look; fi)"
```

- If the `grep` prints a `regolith.look.path` line, your look is already pinned and nothing changes.
- Otherwise `regolith-look set` runs and the screen refreshes. Lines such as "Error: No default value provided" are harmless. Afterwards, `~/.config/regolith3/Xresources` contains `regolith.look.path: /usr/share/regolith-look/<your look>`.
- The command reads the look with `trawlcat` in the Wayland session, because `xrescat` fails there ("Can't obtain RESOURCE_MANAGER").

### 3. Remove the stale sway-audio-idle-inhibit link

```bash
sudo rm -fv /etc/systemd/user/default.target.wants/sway-audio-idle-inhibit.service
```

On Regolith 3.4 this prints `removed '/etc/systemd/user/default.target.wants/sway-audio-idle-inhibit.service'`. On Regolith 3.3 it prints nothing.

The `sway-audio-idle-inhibit` 2.0.0 package in Regolith 3.4 enables itself for every user, including the login screen's own user. If this link is still there when you first boot Ubuntu 26.04 with the old package, the login screen fails to start and you get a black screen ([fix](#black-screen-instead-of-the-login-screen)). Removing it costs nothing: before the upgrade, the link only makes `sway-audio-idle-inhibit` crash in X11 sessions (the "System program problem detected" dialogs). Step 5 installs a newer version, which runs only in the Regolith Wayland session, and turns it back on.

### 4. Run the release upgrade from a text console

1. Save your work and log out.
1. At the login screen, press {{< keys "ctrl,alt,f3" >}} and log in on the text console.
1. Run:

   ```bash
   sudo do-release-upgrade
   ```

Notes:
- Don't run the upgrade from a terminal inside the Regolith session: the packages of that session are replaced while it runs.
- The upgrader runs inside `screen`. If you lose the console, log in again and run `sudo screen -x` to reattach. Logs are in `/var/log/dist-upgrade/`.
- Over ssh, the upgrader offers to start a second ssh server on port 1022.

Answer the upgrader's questions as follows. The package counts depend on your system.

| Question (abridged) | Answer | Notes |
|---|---|---|
| Release notes … `Continue [yN]` | **y** | |
| **Foreign Packages Installed:** `regolith-powerd … Installed from: Regolith` … "Do you want to continue the upgrade anyways?" | **y** | Ubuntu 26.04 has an older package with the same name. Regolith's version is kept. |
| "Do you want to start the upgrade? … packages are going to be removed …" `Continue [yN] Details [d]` | **y** | The removals include Regolith's Settings app (`regolith-control-center`, `regolith-i3-control-center-regolith`, `regolith-sway-control-center-regolith`) and `xdg-desktop-portal-regolith` if you have it. Coming from Regolith 3.3, they also include `xdg-desktop-portal-regolith-x11-config` and `xdg-desktop-portal-regolith-wayland-config`. Step 5 installs them again. |
| *(sometimes)* "Lock screen disabled … To continue please press [ENTER]" | **Enter** | Appears if a graphical session was open earlier in this boot. |
| "Configuring kdump-tools: Should kdump-tools be enabled by default?" | **Enter** | Ubuntu's crash-dump service. Either answer works. |
| "Remove obsolete packages? … packages are going to be removed." `Continue [yN]` | **N** | Keep everything until both sessions work; step 8 cleans up. |
| "System upgrade is complete. Restart required … Continue [yN]" | **N** | Don't reboot yet. Do step 5 first, on the same console. |

If a configuration file question appears (`*** file (Y/I/N/O/D/Z) [default=N] ?`) for a file you edited, press **N** to keep your version. For a file you never touched, **Y** is fine.

If the upgrader asks "Required apt sources are missing … Would you like to continue the upgrade using default sources?", a download of the 26.04 package lists failed (for example a dropped connection). Answer **N**. The upgrader restores your system and stops; press **x** to close its `Command terminated` screen. Then follow [The upgrader stopped before installing anything](#the-upgrader-stopped-before-installing-anything) and run this step again.

### 5. Point Regolith at Ubuntu 26.04 (same console, before rebooting)

The upgrader replaced `regolith.list` with a disabled copy (`regolith.list.disabled`). Replace it with a source for `testing resolute`, then upgrade the Regolith packages and install the ones the upgrader removed:

```bash
sudo rm -f /etc/apt/sources.list.d/regolith.list /etc/apt/sources.list.d/regolith.list.disabled /etc/apt/sources.list.d/regolith.list.distUpgrade
sudo tee /etc/apt/sources.list.d/regolith.sources > /dev/null <<'EOF'
Types: deb
URIs: https://archive.regolith-desktop.com/ubuntu/testing
Suites: resolute
Components: main
Architectures: amd64
Signed-By: /usr/share/keyrings/regolith-archive-keyring.gpg
EOF
sudo apt update
sudo apt full-upgrade
sudo apt install regolith-desktop regolith-session-flashback regolith-session-sway regolith-look-lascaille regolith-control-center regolith-i3-control-center-regolith regolith-sway-control-center-regolith xdg-desktop-portal-regolith-x11-config xdg-desktop-portal-regolith-wayland-config
```

- Use `full-upgrade`, not `upgrade`: plain `apt upgrade` never removes packages and can keep Regolith packages back ([Discussion #1154](https://github.com/orgs/regolith-linux/discussions/1154)).
- In the `apt install` line, replace `regolith-look-lascaille` with the look you use, and leave out `regolith-session-sway` if you don't use the Wayland session. Add any other Regolith packages you installed yourself, or let step 6 find them. Packages that are already installed are left as they are.
- Answer **Y** to apt's "Do you want to continue?" questions.
- `full-upgrade` updates `/etc/regolith/version` and may print "Pending kernel upgrade!" and a list of services to restart. That is expected: you reboot in step 7.

{{< callout type="info" >}}
On ARM-based systems, replace `amd64` with `arm64` in the `Architectures:` line.
{{< /callout >}}

Turn `sway-audio-idle-inhibit` back on. It keeps the Wayland session from locking or blanking the screen while sound plays:

```bash
sudo systemctl --global enable sway-audio-idle-inhibit.service
```

If you removed the link in step 3, this prints `Created symlink '/etc/systemd/user/graphical-session.target.wants/sway-audio-idle-inhibit.service' → …`. Otherwise it prints nothing, because the package has already enabled itself.

Check that the Settings app (opened with {{< keys "super,c" >}}) and the portal configuration are installed:

```bash
dpkg-query -W -f='${db:Status-Abbrev} ${Package} ${Version}\n' regolith-desktop regolith-control-center regolith-i3-control-center-regolith regolith-sway-control-center-regolith xdg-desktop-portal-regolith-x11-config xdg-desktop-portal-regolith-wayland-config
```

All six lines should start with `ii`. The two `xdg-desktop-portal-regolith-*-config` packages route file choosers, screenshots and screen sharing to the GTK, GNOME and wlroots portals.

**Old portal backend.** If you had `xdg-desktop-portal-regolith` (older versions of the install page installed it), the upgrader removes it. Don't install it again: this backend is deprecated and being retired, and nothing on Ubuntu 26.04 uses it.

### 6. Check that nothing you had is missing

```bash
comm -23 <(awk '{print $1}' ~/upgrade-backup/regolith-packages-before.txt | sort) <(dpkg-query -W -f='${db:Status-Abbrev} ${Package}\n' | awk '$1=="ii"{print $2}' | sort)
```

This lists the Regolith-related packages you had before the upgrade that are no longer installed. If it prints any, install them with `sudo apt install <names>`, except these expected ones:
- `xdg-desktop-portal-regolith`: deprecated (see **Old portal backend** in step 5).
- `regolith-sway-audio-idle-inhibit` (Regolith 3.3): replaced by `regolith-session-sway`.
- Packages that only existed for your old release and have a successor: for example `libwlroots-0.18` is replaced by `libwlroots-0.19`.

### 7. Reboot

```bash
sudo reboot
```

The login screen should appear as usual. Log in once to **Regolith on X11** and once to **Regolith on Wayland** (if you use it), using the gear icon on the password screen, and go through the [checks](#check-the-result).

### 8. Clean up

Once both sessions work, remove the packages that Ubuntu 26.04 no longer needs:

```bash
sudo apt autoremove --purge
```

If it lists a Regolith package that you use, answer **n**, install that package with `sudo apt install <name>`, and run it again.

The kernel of your old release stays installed as a fallback. Once the new kernel has worked for a few days, list the old kernel packages, review the list, and remove them:

{{< tabs items="Ubuntu 25.10 (kernel 6.17),Ubuntu 25.04 (kernel 6.14)" >}}
{{< tab >}}
```bash
dpkg -l 'linux-*6.17.0*' | awk '/^ii/{print $2}'
```

```bash
sudo apt purge $(dpkg -l 'linux-*6.17.0*' | awk '/^ii/{print $2}')
```
{{< /tab >}}
{{< tab >}}
```bash
dpkg -l 'linux-*6.14.0*' | awk '/^ii/{print $2}'
```

```bash
sudo apt purge $(dpkg -l 'linux-*6.14.0*' | awk '/^ii/{print $2}')
```
{{< /tab >}}
{{< /tabs >}}

Re-enable your third-party repositories and PPAs one at a time, once each publishes packages for `resolute`: set `Enabled: yes` in its `.sources` file, or add the PPA again.

## Check the result

In both sessions:

| Check | How | Expected |
|---|---|---|
| Terminal | {{< keys "super,enter" >}} | A terminal opens. |
| Launcher | {{< keys "super,space" >}}, type "calc", Enter | ilia opens and starts Calculator. |
| Notifications | `notify-send test hello`, then {{< keys "super,n" >}} | The notification appears, and the notification viewer opens. |
| Bar | Look at the bottom of the screen | X11: i3bar with i3xrocks. Wayland: swaybar with i3status-rs. |
| Lock screen | {{< keys "super,escape" >}}, then type your password | The screen locks and unlocks. |
| Settings | {{< keys "super,c" >}} | The Settings app opens, including Displays, Network, Sound and Power. |
| Volume and brightness keys | Press them | The level changes, and an on-screen indicator shows it. |
| Look | {{< keys "super,alt,l" >}}, pick another look, Enter | The look changes. |
| Suspend | `systemctl suspend`, then wake the machine | It resumes to the lock screen, and the password unlocks it. |

On a laptop, also check Wi-Fi (`nmcli device`), the backlight (`ls /sys/class/backlight/`), external monitors and closing the lid.

Then check apt:

```bash
lsb_release -ds
cat /etc/regolith/version
sudo apt update
sudo apt-get -s full-upgrade | tail -1
apt-mark showhold
sudo dpkg --audit
apt policy regolith-desktop | head -4
```

What to expect:
- `Ubuntu 26.04.1 LTS` (or a later point release) and `REGOLITH_VERSION=3.5`.
- `apt update` without errors or warnings. `apt-mark showhold` and `dpkg --audit` print nothing.
- `apt policy` shows the same `Installed` and `Candidate` versions. The "WARNING: apt does not have a stable CLI interface" line is normal.
- The `full-upgrade` simulation ends with `0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.` If it shows other lines, `testing` has published updates since step 5. Install them with `sudo apt full-upgrade`, then repeat the check.

Some packages that both Ubuntu and Regolith ship, such as `trawld`, `gtklock`, `i3-wm` and `swayidle`, can come from Ubuntu rather than from the Regolith repository when Ubuntu 26.04 has the newer version (check with `apt policy <package>`). This is expected; don't pin the Regolith versions.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| <a id="black-screen-instead-of-the-login-screen"></a>**Black screen instead of the login screen** on the first 26.04 boot | The `sway-audio-idle-inhibit` link from step 3 is still there, and the reboot happened before step 5. | Press {{< keys "ctrl,alt,f3" >}}, log in, run the command of [step 3](#3-remove-the-stale-sway-audio-idle-inhibit-link), and run `sudo systemctl restart gdm3`. Then do step 5. |
| `apt` finds no Regolith packages, and `regolith.list.disabled` is in `/etc/apt/sources.list.d/` | The upgrader disables every third-party repository. | Step 5. |
| {{< keys "super,c" >}} does nothing | The upgrader removed the Settings app. | The `apt install` line of step 5. |
| The look changed after an update | No look was pinned. | `regolith-look set <your look>` (step 2). |
| `do-release-upgrade` says "Please install all available updates for your release before upgrading." | Updates for your current release are pending. | `sudo apt update && sudo apt full-upgrade`, then try again. |
| `do-release-upgrade` offers 25.10 instead of 26.04 | You used `-d` on 25.04. | Run it without `-d`. |
| `apt upgrade` says "The following packages have been kept back" for Regolith packages | Plain `upgrade` doesn't remove or replace packages. | Use `sudo apt full-upgrade`. |
| Applications from PPAs are missing or old | The upgrader disabled all PPAs. | Re-enable them once they publish for `resolute` (step 8). |
| The console upgrade seems frozen, or you lost the console | It runs inside `screen`. | `sudo screen -x`. Logs are in `/var/log/dist-upgrade/`. |
| "System program problem detected" for `sway-audio-idle-inhibit`, once per Wayland login, when sound first plays | Known issue in the current `testing` package (2.0.3): it stops once with "Could not send inhibit signal!", and is restarted 5 seconds later. | Dismiss the dialog. It works normally after the restart. |

## Recovery

There is no supported way back from 26.04 to your old release. Your backup is the real rollback. The cases below get you to a working system.

### The upgrader stopped before installing anything

This happens when you answer **N** to "Required apt sources are missing", or when the upgrader stops with "Could not calculate the upgrade" or a network error. It restores your old Ubuntu sources, but leaves the Regolith repository disabled. Put your Regolith line from [Check where you are](#check-where-you-are) back, then run step 4 again:

{{< tabs items="Ubuntu 25.10,Ubuntu 25.04" >}}
{{< tab >}}
```bash
sudo rm -f /etc/apt/sources.list.d/regolith.list.disabled /etc/apt/sources.list.d/regolith.list.distUpgrade
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg] https://archive.regolith-desktop.com/ubuntu/stable questing v3.4" | sudo tee /etc/apt/sources.list.d/regolith.list
sudo apt update
```
{{< /tab >}}
{{< tab >}}
```bash
sudo rm -f /etc/apt/sources.list.d/regolith.list.disabled /etc/apt/sources.list.d/regolith.list.distUpgrade
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg] https://archive.regolith-desktop.com/ubuntu/stable plucky v3.3" | sudo tee /etc/apt/sources.list.d/regolith.list
sudo apt update
```

If you have Regolith 3.4, write `v3.4` instead of `v3.3`.
{{< /tab >}}
{{< /tabs >}}

`apt update` should list the `stable` Regolith suite again, without errors.

### The upgrade stopped halfway

For example after a power loss or a crash. From a text console (press {{< keys "ctrl,alt,f3" >}}), or from the recovery mode root shell after `mount -o remount,rw /`:

```bash
sudo dpkg --configure -a
sudo apt -f install
sudo apt full-upgrade
sudo do-release-upgrade        # if it still offers 26.04, it finishes the job
```

Check that `/etc/apt/sources.list.d/ubuntu.sources` says `resolute`. Look in `/var/log/dist-upgrade/main.log` and `apt.log` for the package that failed.

### You rebooted before step 5

If step 3 was done, the login screen appears and both sessions start with your old Regolith packages, without the Settings app; Ubuntu's setup wizard may open. Log in, open a terminal or a text console, and do steps 5 to 8. If you skipped step 3, see [black screen](#black-screen-instead-of-the-login-screen).

### Regolith doesn't start, or you are sent back to the login screen

Log in to **Ubuntu** from the gear menu, or use a text console, then:

```bash
cat /etc/apt/sources.list.d/regolith.sources            # testing, resolute, main
sudo apt update && sudo apt full-upgrade
sudo apt install --reinstall regolith-session-common regolith-session-flashback regolith-session-sway regolith-wm-config sway-regolith
journalctl --user -b -p warning | tail -50              # as your user, after a failed login
```

Leave out `regolith-session-sway` and `sway-regolith` if you don't use the Wayland session.

- To rule out your own configuration, run `mv ~/.config/regolith3 ~/.config/regolith3.off`, log in again, and restore pieces from the backup.
- If the login screen itself fails, run `sudo systemctl restart gdm3`, and check that no `sway-audio-idle-inhibit.service` link is left in `/etc/systemd/user/default.target.wants/`.

### Black screen with an NVIDIA GPU

From a text console, run `sudo ubuntu-drivers install` (or reinstall your `nvidia-driver-NNN` package), then `sudo update-initramfs -u`, and reboot. You can also start the kernel of your old release (6.17 on 25.10, 6.14 on 25.04) from GRUB's "Advanced options" menu, as long as step 8 hasn't removed it: hold Shift or press Esc while the machine boots.

## Move to the stable release later

When Regolith 3.5 is released, the [install page](/docs/using-regolith/install/) will list the repository line for Ubuntu 26.04. Switching from `testing` to it is a change of source, because `testing` is built from the same release tags:

1. Look up the suite and component for Ubuntu 26.04 on the install page. The examples below assume `stable resolute v3.5`.
1. Edit the source and upgrade:

   ```bash
   sudo sed -i -e 's#/ubuntu/testing#/ubuntu/stable#' -e 's#^Components: .*#Components: v3.5#' /etc/apt/sources.list.d/regolith.sources
   sudo apt update
   sudo apt full-upgrade
   ```

If the stable release has lower version numbers than some packages you have from `testing`, `full-upgrade` leaves those packages alone. To move them to the stable versions too, give the Regolith repository priority for one upgrade, then remove the pin and upgrade again:

```bash
printf 'Package: *\nPin: origin archive.regolith-desktop.com\nPin-Priority: 1001\n' | sudo tee /etc/apt/preferences.d/regolith-switch.pref
sudo apt update && sudo apt full-upgrade --allow-downgrades
sudo rm /etc/apt/preferences.d/regolith-switch.pref
sudo apt update && sudo apt full-upgrade
```

The last `full-upgrade` is needed: while the pin is active, apt also downgrades Ubuntu packages that Regolith also ships (such as `trawld` and `gtklock`), and this puts Ubuntu's newer versions back.

Afterwards, `apt policy regolith-desktop` should show the installed version coming from `https://archive.regolith-desktop.com/ubuntu/stable`.

### Why `testing` and not `unstable`

- `unstable` is rebuilt from the development branches after every change, with snapshot version numbers (`…regolith+git<date>…`). These sort above the released versions, so you would get untested changes, and leaving `unstable` later means downgrading.
- Moving from `testing` to the stable release is a change of source. Moving from `unstable` needs the downgrade procedure above.
