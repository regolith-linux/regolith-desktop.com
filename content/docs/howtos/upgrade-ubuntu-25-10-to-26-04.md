---
title: "Upgrade from Ubuntu 25.10 to 26.04"
linkTitle: "Upgrade from Ubuntu 25.10 to 26.04"
description: >
  Upgrade an Ubuntu 25.10 system running Regolith 3.4 to Ubuntu 26.04 LTS
---

This page takes an Ubuntu 25.10 (Questing) system with Regolith 3.4, installed as described on the [install page](/docs/using-regolith/install/), through Ubuntu's `do-release-upgrade` to Ubuntu 26.04 LTS (Resolute), and then brings Regolith back.

The Ubuntu upgrader disables the Regolith package repository and removes a few Regolith packages, so some extra steps are needed before and after it runs. Do them in the order below.

{{< callout type="warning" >}}
Regolith has no stable release for Ubuntu 26.04 yet: `https://archive.regolith-desktop.com/ubuntu/stable` has no `resolute` suite. Until Regolith 3.5 is released, this page uses the **`testing`** suite, which is built from the same release tags that the stable release will use. [Move to the stable suite](#move-to-the-stable-release-later) once the install page lists it for Ubuntu 26.04.
{{< /callout >}}

## Overview

1. Install all 25.10 updates and remove one stale systemd link.
1. Run `sudo do-release-upgrade` from a text console.
1. When the upgrader asks to restart, answer **N**.
1. On the same console, point Regolith at `testing resolute` and upgrade the Regolith packages.
1. Reboot, log in to both Regolith sessions, and clean up.

The download is about 2.3 GB and the upgrade itself takes about an hour on a fast machine. On a laptop, plan 1.5 to 3 hours, and keep it on AC power.

## Before you start

### Check where you are

```bash
lsb_release -ds                      # Ubuntu 25.10
cat /etc/regolith/version            # REGOLITH_VERSION=3.4
ls /etc/apt/sources.list.d/
cat /etc/apt/sources.list.d/regolith.list
```

A system installed from the install page has `/etc/apt/sources.list.d/regolith.list` with the line `deb [arch=amd64 signed-by=/usr/share/keyrings/regolith-archive-keyring.gpg] https://archive.regolith-desktop.com/ubuntu/stable questing v3.4`, and the key in `/usr/share/keyrings/regolith-archive-keyring.gpg`. The key stays valid for Ubuntu 26.04 and is reused below.

If you already use the `testing` or `unstable` suite on 25.10, the same steps work. Your source file may be called `regolith.sources` instead of `regolith.list`; step 5 replaces either one. If you added the Regolith repository under any other file name, delete that file in step 5, so that the repository isn't listed twice.

### Checklist

| Check | Command | What to do |
|---|---|---|
| Free disk space | `df -h / /boot` | Have at least 6 GB free on `/` and 300 MB on `/boot`. |
| Third-party repositories and PPAs | `ls /etc/apt/sources.list.d/` | The upgrader disables all of them. Write them down, so you can re-enable each one once it publishes packages for `resolute`. |
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
```

Step 6 uses the package list to find anything that the upgrade removed and didn't bring back.

## Steps

### 1. Install all 25.10 updates

```bash
sudo apt update
sudo apt full-upgrade
[ -f /var/run/reboot-required ] && echo "reboot first" || echo "no reboot needed"
```

Reboot if it says so. `do-release-upgrade` refuses to start while updates are pending.

Ubuntu 25.10 has reached its end of life:
- Ignore the "Software updates are no longer provided" dialog, and don't use its upgrade button. Use the console steps below instead.
- If `apt update` fails with `404 Not Found` errors for `questing`, the 25.10 archive has moved to `old-releases.ubuntu.com`. Point apt there, then continue (the upgrader switches back to the normal mirror):

  ```bash
  sudo sed -i 's#http://\(archive\|security\).ubuntu.com#http://old-releases.ubuntu.com#' /etc/apt/sources.list.d/ubuntu.sources
  sudo apt update
  ```

### 2. Pin your look (recommended)

If you never chose a look with `regolith-look set`, a later package update can switch you to another one. Pin the current look, in a Regolith session:

```bash
grep -s regolith.look.path ~/.config/regolith3/Xresources || regolith-look set "$(xrescat regolith.look)"
```

If the `grep` prints a `regolith.look.path` line, your look is already pinned and nothing changes.

### 3. Remove the stale sway-audio-idle-inhibit link, if it exists

```bash
ls -l /etc/systemd/user/default.target.wants/sway-audio-idle-inhibit.service
```

If the file exists, remove it:

```bash
sudo rm /etc/systemd/user/default.target.wants/sway-audio-idle-inhibit.service
```

The `sway-audio-idle-inhibit` 2.0.0 package in Regolith 3.4 enables itself for every user, including the login screen's own user. If this link is still there when you first boot Ubuntu 26.04 with the old package, the login screen fails to start and you get a black screen ([fix](#black-screen-instead-of-the-login-screen)). Removing it costs nothing: on 25.10 the link only makes `sway-audio-idle-inhibit` crash in X11 sessions (the "System program problem detected" dialogs). Newer versions of the package, which step 5 installs, are started by the session instead.

### 4. Run the release upgrade from a text console

1. Save your work and log out.
1. At the login screen, press {{< keys "ctrl,alt,f3" >}} and log in on the text console.
1. Run:

   ```bash
   sudo do-release-upgrade
   ```

Notes:
- Ubuntu 26.04 LTS is offered without `-d` since 26.04.1. If it says "No new release found", check `https://changelogs.ubuntu.com/meta-release` and add `-d` only if it lists resolute as supported.
- Don't run the upgrade from a terminal inside the Regolith session: the packages of that session are replaced while it runs.
- The upgrader runs inside `screen`. If you lose the console, log in again and run `sudo screen -x` to reattach. Logs are in `/var/log/dist-upgrade/`.
- Over ssh, the upgrader offers to start a second ssh server on port 1022.

Answer the upgrader's questions as follows. The package counts depend on your system.

| Question (abridged) | Answer | Notes |
|---|---|---|
| Release notes … `Continue [yN]` | **y** | |
| **Foreign Packages Installed:** `regolith-powerd … Installed from: Regolith` … "Do you want to continue the upgrade anyways?" | **y** | Ubuntu 26.04 has an older package with the same name. Regolith's version is kept. |
| "Do you want to start the upgrade? … packages are going to be removed …" `Continue [yN] Details [d]` | **y** | The removals include `regolith-control-center`, `regolith-i3-control-center-regolith`, `regolith-sway-control-center-regolith` and `xdg-desktop-portal-regolith`. Step 5 deals with them. |
| *(sometimes)* "Lock screen disabled … To continue please press [ENTER]" | **Enter** | Appears if a graphical session was open earlier in this boot. |
| "Configuring kdump-tools: Should kdump-tools be enabled by default?" | **Enter** | Ubuntu's crash-dump service. Either answer works. |
| "Remove obsolete packages? … packages are going to be removed." `Continue [yN]` | **N** | Keep everything until both sessions work; step 8 cleans up. |
| "System upgrade is complete. Restart required … Continue [yN]" | **N** | Don't reboot yet. Do step 5 first, on the same console. |

If a configuration file question appears (`*** file (Y/I/N/O/D/Z) [default=N] ?`) for a file you edited, press **N** to keep your version. For a file you never touched, **Y** is fine.

### 5. Point Regolith at Ubuntu 26.04 (same console, before rebooting)

The upgrader replaced `regolith.list` with a disabled copy (`regolith.list.disabled`). Replace it with a source for `testing resolute`, then upgrade the Regolith packages:

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
sudo apt install regolith-desktop regolith-session-flashback regolith-look-lascaille
```

- Use `full-upgrade`, not `upgrade`: plain `apt upgrade` never removes packages and can keep Regolith packages back ([Discussion #1154](https://github.com/orgs/regolith-linux/discussions/1154)).
- In the `apt install` line, replace `regolith-look-lascaille` with the look you use, and add `regolith-session-sway` if you use the Wayland session. Add any other Regolith packages you installed yourself, or let step 6 find them.
- Answer **Y** to apt's "Do you want to continue?" questions.

{{< callout type="info" >}}
On ARM-based systems, replace `amd64` with `arm64` in the `Architectures:` line.
{{< /callout >}}

**Settings app.** If any of the following packages is not listed with `ii`, or `dpkg-query` reports that it finds no such package, the Settings app ({{< keys "super,c" >}}) is missing:

```bash
dpkg-query -W -f='${db:Status-Abbrev} ${Package}\n' regolith-control-center regolith-i3-control-center-regolith regolith-sway-control-center-regolith
```

In that case, install it:

```bash
sudo apt install regolith-control-center regolith-i3-control-center-regolith regolith-sway-control-center-regolith
```

**Portal backend.** The upgrader removes `xdg-desktop-portal-regolith`. Don't install it again: this backend is deprecated and being retired, and nothing on Ubuntu 26.04 uses it. File choosers, screenshots and screen sharing are provided by the GTK, GNOME and wlroots portals, which the session packages configure through `xdg-desktop-portal-regolith-x11-config` and `xdg-desktop-portal-regolith-wayland-config`. These configuration packages are kept by the upgrade.

### 6. Check that nothing you had is missing

```bash
comm -23 <(awk '{print $1}' ~/upgrade-backup/regolith-packages-before.txt | sort) <(dpkg-query -W -f='${db:Status-Abbrev} ${Package}\n' | awk '$1=="ii"{print $2}' | sort)
```

This lists the Regolith-related packages you had before the upgrade that are no longer installed. If it prints any, install them with `sudo apt install <names>`, with two exceptions:
- `xdg-desktop-portal-regolith` is expected in the list. Leave it out (see **Portal backend** in step 5).
- Packages that only existed for 25.10 and have a successor: for example `libwlroots-0.18` is replaced by `libwlroots-0.19`.

### 7. Reboot

```bash
sudo reboot
```

The login screen should appear as usual. Log in once to **Regolith on X11** and once to **Regolith on Wayland** (if you use it), using the gear icon on the password screen, and go through the [checks](#check-the-result).

**Ubuntu's "What's new" window.** If a "Welcome" / "What's new in Ubuntu 26.04" window opens when you log in to Regolith, click through it to **Finish** once. It is Ubuntu's upgrade wizard and keeps coming back at every login until you finish it. You can also dismiss it for good from a terminal:

```bash
mkdir -p ~/.config/gnome-initial-setup && touch ~/.config/gnome-initial-setup/upgrade-26.04-done
```

### 8. Clean up

Once both sessions work, remove the packages that Ubuntu 26.04 no longer needs:

```bash
sudo apt autoremove --purge
```

The 25.10 kernel (6.17) stays installed as a fallback. Once the new kernel has worked for a few days, remove it:

```bash
dpkg -l 'linux-*6.17.0*' | awk '/^ii/{print $2}'      # review the list, then:
sudo apt purge $(dpkg -l 'linux-*6.17.0*' | awk '/^ii/{print $2}')
```

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
| Volume and brightness keys | Press them | The level changes. |
| Suspend | `systemctl suspend`, then wake the machine | It resumes to the lock screen, and the password unlocks it. |

On a laptop, also check Wi-Fi (`nmcli device`), the backlight (`ls /sys/class/backlight/`), external monitors and closing the lid.

Then check apt:

```bash
sudo apt update                          # no errors or warnings
apt list --upgradable                    # empty
sudo apt-get -s full-upgrade | tail -1   # 0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
sudo dpkg --audit                        # empty
cat /etc/apt/sources.list.d/regolith.sources
```

What to expect on the `testing` suite:
- `cat /etc/regolith/version` still says `REGOLITH_VERSION=3.4`. That is expected until Regolith 3.5 is released.
- Some packages that both Ubuntu and Regolith ship, such as `trawld`, `gtklock`, `i3-wm` and `swayidle`, can come from Ubuntu rather than from the Regolith repository when Ubuntu 26.04 has the newer version (check with `apt policy <package>`). This is expected; don't pin the Regolith versions.

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| <a id="black-screen-instead-of-the-login-screen"></a>**Black screen instead of the login screen** on the first 26.04 boot | The `sway-audio-idle-inhibit` link from step 3 is still there, and the reboot happened before step 5. | Press {{< keys "ctrl,alt,f3" >}}, log in, and run `sudo rm -f /etc/systemd/user/default.target.wants/sway-audio-idle-inhibit.service && sudo systemctl restart gdm3`. Then do step 5. |
| `apt` finds no Regolith packages, and `regolith.list.disabled` is in `/etc/apt/sources.list.d/` | The upgrader disables every third-party repository. | Step 5. |
| {{< keys "super,c" >}} does nothing | The upgrader removed the Settings app. | The Settings app part of step 5. |
| X11 fonts and windows look smaller: your `Xft.dpi` setting is ignored | The X11 session on the `testing` suite starts GNOME's settings daemon, which sets the DPI to 96. | If this happens, set GNOME's text scaling to your DPI divided by 96, for example `gsettings set org.gnome.desktop.interface text-scaling-factor 1.15` for 110 DPI. |
| The look changed after an update | No look was pinned. | `regolith-look set <your look>` (step 2). |
| The volume changes in the Wayland session, but no volume bubble appears | A bug in the `regolith-avizo` package on the `testing` suite; 25.10 with Regolith 3.4 has it too. | Cosmetic: the bar shows the level. It goes away with a newer `regolith-avizo` release. |
| `do-release-upgrade` says "Please install all available updates for your release before upgrading." | 25.10 updates are pending. | `sudo apt update && sudo apt full-upgrade`, then try again. |
| `apt upgrade` says "The following packages have been kept back" for Regolith packages | Plain `upgrade` doesn't remove or replace packages. | Use `sudo apt full-upgrade`. |
| Applications from PPAs are missing or old | The upgrader disabled all PPAs. | Re-enable them once they publish for `resolute` (step 8). |
| The console upgrade seems frozen, or you lost the console | It runs inside `screen`. | `sudo screen -x`. Logs are in `/var/log/dist-upgrade/`. |

## Recovery

There is no supported way back from 26.04 to 25.10. Your backup is the real rollback. The cases below get you to a working 26.04 system.

**The upgrade stopped halfway** (power loss, network failure, an error). From a text console ({{< keys "ctrl,alt,f3" >}}), or from the recovery mode root shell after `mount -o remount,rw /`:

```bash
sudo dpkg --configure -a
sudo apt -f install
sudo apt full-upgrade
sudo do-release-upgrade        # if it still offers 26.04, it finishes the job
```

- Check that `/etc/apt/sources.list.d/ubuntu.sources` says `resolute`. Look in `/var/log/dist-upgrade/main.log` and `apt.log` for the package that failed.
- If the upgrader stopped with "Could not calculate the upgrade", it restores the 25.10 sources but leaves the Regolith repository disabled. Restore `regolith.list` from `~/upgrade-backup/etc-regolith-and-apt.tgz`, or add it again from the [install page](/docs/using-regolith/install/), before you try again.

**You rebooted before step 5.** If you removed the link in step 3, the login screen appears and both sessions start with the old Regolith 3.4 packages, without the Settings app. Log in, open a terminal or a text console, and do step 5. If you skipped step 3, see [black screen](#black-screen-instead-of-the-login-screen).

**Regolith doesn't start, or you are sent back to the login screen.** Log in to **Ubuntu** from the gear menu, or use a text console, then:

```bash
cat /etc/apt/sources.list.d/regolith.sources            # testing, resolute, main
sudo apt update && sudo apt full-upgrade
sudo apt install --reinstall regolith-session-common regolith-session-flashback regolith-session-sway regolith-wm-config sway-regolith
journalctl --user -b -p warning | tail -50              # as your user, after a failed login
```

Leave out `regolith-session-sway` and `sway-regolith` if you don't use the Wayland session.

- To rule out your own configuration, run `mv ~/.config/regolith3 ~/.config/regolith3.off`, log in again, and restore pieces from the backup.
- If the login screen itself fails, run `sudo systemctl restart gdm3`, and check that no `sway-audio-idle-inhibit.service` link is left in `/etc/systemd/user/default.target.wants/`.

**Black screen with an NVIDIA GPU.** From a text console, run `sudo ubuntu-drivers install` (or reinstall your `nvidia-driver-NNN` package), then `sudo update-initramfs -u`, and reboot. You can also start the previous 6.17 kernel from GRUB's "Advanced options" menu: hold Shift or press Esc while the machine boots.

## Move to the stable release later

When Regolith 3.5 is released, the [install page](/docs/using-regolith/install/) will list the repository line for Ubuntu 26.04. Switching from `testing` to it is a change of source, because `testing` is built from the same release tags:

1. Look up the suite and component for Ubuntu 26.04 on the install page. Ubuntu 25.10 uses `stable questing v3.4`; the examples below assume `stable resolute v3.5`.
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

Afterwards, `cat /etc/regolith/version` should say `REGOLITH_VERSION=3.5`.

### Why `testing` and not `unstable`

- `unstable` is rebuilt from the development branches, and rebuilds can be published under unchanged version numbers. An installed system never receives those later rebuilds, because apt sees no new version, so what you get depends on the day you installed.
- Moving from `testing` to the stable release is a change of source. Moving from `unstable` needs the downgrade procedure above.
