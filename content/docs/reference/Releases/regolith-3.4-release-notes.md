---
title: "3.4 Release Notes"
linkTitle: "3.4 Release Notes"
weight: 83
description: >
  Release notes for Regolith 3.3
prev: /docs/reference/releases
---

## Regolith 3.4 Release Notes

Regolith 3.4 is a minor release focusing on Ubuntu 25.10 and Debian 13 support.

## Features

* Ubuntu 25.10 (Questing) support
* Debian 13 (Trixie) support

## Known Issues

TBA

## Installation Instructions

Refer to the [installation instructions](/docs/using-regolith/install/) to install or upgrade from a previous version.

After the Regolith upgrade is complete, verify that Regolith 3.4 is installed by:

```console
$ cat /etc/regolith/version 
REGOLITH_VERSION=3.4
```

## Changes since Regolith 3.3

### armesto <sup>[link](https://github.com/regolith-linux/armesto)</sup>

#### main <sup>[link](https://github.com/regolith-linux/armesto/tree/main)</sup>
- fix: use correct version spec to avoid newer, rustc-incompat dependencies <sup>[link](https://github.com/regolith-linux/armesto/commit/3bbb44e15c792374cd02b44fc06a9257789f3e04)</sup>

### i3status-rs <sup>[link](https://github.com/regolith-linux/i3status-rs_debian)</sup>
#### ubuntu/0.34.0 <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/tree/ubuntu/0.34.0)</sup>
- Package Changes <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/commit/bf5f37ba371df6052b97e0de63ed981a2bd11a73)</sup>
- v0.34.0 <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/commit/db4db28eda9bbf8b7fc61ddad65a00207be9368b)</sup>
- clippy: variables can be used directly in the `format!` string <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/commit/1749515d23cb3aead7e96749626798c806f711bf)</sup>
- Update NEWS.md <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/commit/c8b2fb2f5e81d2e9a2adc8bc197747eaf04750da)</sup>
- blocks[battery[apc_ups]]: Deserialize APC UPS data <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/commit/4f06b21ef66e88bef4929e5809b054a21bcc7ef4)</sup>
- [deps]: Update swayipc-async <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/commit/fd6b17a14ada5fc6c9c1c6265bd8319486271f4c)</sup>
- [deps]: upgrade calibright, dirs. update shellexpand <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/commit/25ce96d17e2b7ca5817794e2be6fcdfe6b46fa9f)</sup>
- deps[sunrise]: bump to version 2.1, take 2 <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/commit/50c611e1a6bff779283820bbdb9758c5c4c9313a)</sup>
- deps[sunrise]: bump to version 2.1 <sup>[link](https://github.com/regolith-linux/i3status-rs_debian/commit/544d855e756d16b621fcc762dc85f075b29e43f9)</sup>

### ilia <sup>[link](https://github.com/regolith-linux/ilia)</sup>

#### main <sup>[link](https://github.com/regolith-linux/ilia/tree/main)</sup>
- Clean up leftover prints from debugging <sup>[link](https://github.com/regolith-linux/ilia/commit/4822ffac01e3a399a8356134956598de6c3a2347)</sup>
- Force remove sp_after_sparen <sup>[link](https://github.com/regolith-linux/ilia/commit/658720439d8b92bbeceb0f029a43c6ddba8a89d7)</sup>
- Run uncrustify again <sup>[link](https://github.com/regolith-linux/ilia/commit/41d1cefe7faec57dcd24da175f874b757b52f8ca)</sup>
- Default to using release build types <sup>[link](https://github.com/regolith-linux/ilia/commit/c95be02287b70eb40d67ca032fb1d69e255d4fd8)</sup>
- Uncrustify everything <sup>[link](https://github.com/regolith-linux/ilia/commit/d04df63e9306485e995b42475b499bf76c80e36c)</sup>
- Fix screenshot link in readme <sup>[link](https://github.com/regolith-linux/ilia/commit/5d1a89373be8c6f302a8dbc12c2a597a44a856c4)</sup>
- Added debug instructions and timers <sup>[link](https://github.com/regolith-linux/ilia/commit/9230027f710215d775d0ce9a2afe5f1306aa6833)</sup>
- Load icons async <sup>[link](https://github.com/regolith-linux/ilia/commit/ecbdf9d151d70e98feaca8c9ab8b111ab6205fc2)</sup>
- Fix prefix matching to sort correct <sup>[link](https://github.com/regolith-linux/ilia/commit/7385909af3df86510ebb61a883ace4e9bf2d82b1)</sup>
- Extract app sorting into a dedicated function for testing <sup>[link](https://github.com/regolith-linux/ilia/commit/20d2030b5a79c170ce4c6bc43ecd5fad0a83f8d0)</sup>
- Respect icon size=0 harder <sup>[link](https://github.com/regolith-linux/ilia/commit/687835a9a1769318efb348140e58ea6dccc683c7)</sup>
- Removed newlines in debug() statements <sup>[link](https://github.com/regolith-linux/ilia/commit/d5782f569f54a05146776e926a09250cdcb7f9d3)</sup>

### regolith-control-center <sup>[link](https://github.com/regolith-linux/regolith-control-center)</sup>
#### regolith/49 <sup>[link](https://github.com/regolith-linux/regolith-control-center/tree/regolith/49)</sup>
- feat: enable github actions <sup>[link](https://github.com/regolith-linux/regolith-control-center/commit/640ca28864492bb9536fe9e513e0fd0014147569)</sup>
- Avoid shipping files vended by upstream <sup>[link](https://github.com/regolith-linux/regolith-control-center/commit/a7af25b514b8d667c53dd0f2a8bb571ef9d4c113)</sup>
- remove upstream CI files in subprojects <sup>[link](https://github.com/regolith-linux/regolith-control-center/commit/92d4e5fc991d0ec7eb9d59350aea1cd83eaa5aad)</sup>
- Disable some panels.  Remove upstream CI <sup>[link](https://github.com/regolith-linux/regolith-control-center/commit/bcea955b12addeb6dce3b217a2bf25f7b3e8c8b7)</sup>
- Remove desktop files to avoid duplicates in launcher <sup>[link](https://github.com/regolith-linux/regolith-control-center/commit/b3f1844deb08f572683ba80ccf48ad66065971d3)</sup>
- disable unneeded bins <sup>[link](https://github.com/regolith-linux/regolith-control-center/commit/d3abfb48751975253542f8ecf24d7b6ff3c08ea1)</sup>
- rename files to match project name <sup>[link](https://github.com/regolith-linux/regolith-control-center/commit/b5d94f37c3786c2dc81bfd22f574049111083d0f)</sup>
- disable unneeded apps <sup>[link](https://github.com/regolith-linux/regolith-control-center/commit/91e519c3cd3d5b2321e0e563f6635fcfa23a1be9)</sup>
- Pull debian data from regolith/48 <sup>[link](https://github.com/regolith-linux/regolith-control-center/commit/763627ab6988e9f068dcf44ebf66cd0c684af840)</sup>

### regolith-displayd <sup>[link](https://github.com/regolith-linux/regolith-displayd)</sup>

#### fix/clamshell-config <sup>[link](https://github.com/regolith-linux/regolith-displayd/tree/fix/clamshell-config)</sup>
- fix: display config doesn't apply with disabled outputs <sup>[link](https://github.com/regolith-linux/regolith-displayd/commit/f6afeb431cca04be1710a82afd785a54d60f665c)</sup>

### regolith-powerd <sup>[link](https://github.com/regolith-linux/regolith-powerd)</sup>

#### master <sup>[link](https://github.com/regolith-linux/regolith-powerd/tree/master)</sup>
- fix: use the correct action command for locking <sup>[link](https://github.com/regolith-linux/regolith-powerd/commit/6787a7c9cc666c9070fe10a62a8a12c1d73c3cab)</sup>
- fix: swayidle doesn't resume if timeout action in-flight <sup>[link](https://github.com/regolith-linux/regolith-powerd/commit/1bcbf00abd93e37cf1849b51e5b4bfdeb1f6710d)</sup>
- chore!: update the Xresource key for lock program <sup>[link](https://github.com/regolith-linux/regolith-powerd/commit/6067fe6e1598fa5e7de0fa13e909f7a1e9688411)</sup>
- fix: prevent locking upto 5sec after display off <sup>[link](https://github.com/regolith-linux/regolith-powerd/commit/753d1628cbd86ef729ff049de0e946614a42f981)</sup>
- feat: autolock 5 seconds after idle timeout <sup>[link](https://github.com/regolith-linux/regolith-powerd/commit/79ca7368a58e9b7f463bf40a91fbecc76e207639)</sup>

### regolith-rofication <sup>[link](https://github.com/regolith-linux/regolith-rofication)</sup>

#### master <sup>[link](https://github.com/regolith-linux/regolith-rofication/tree/master)</sup>
- fix: add new dependency in ubuntu-plucky and similar.  Fixes build issue w/ distutils <sup>[link](https://github.com/regolith-linux/regolith-rofication/commit/e0a4d202f19995a0a5f6f0b7fbe73eab67f1087f)</sup>
- Use xrescat in PATH (#21) <sup>[link](https://github.com/regolith-linux/regolith-rofication/commit/997f79b98a603497518d65df893a3327350570bd)</sup>

### regolith-session <sup>[link](https://github.com/regolith-linux/regolith-session)</sup>

#### debian/trixie <sup>[link](https://github.com/regolith-linux/regolith-session/tree/debian/trixie)</sup>
- feat: support trixie in a separate branch until upstream fix (#59) <sup>[link](https://github.com/regolith-linux/regolith-session/commit/ca064d576c071062544a1b9edf313fda0b321f84)</sup>
- cherry pick from main to trixie (#58) <sup>[link](https://github.com/regolith-linux/regolith-session/commit/b7229ccd8437ef578d8a8af995d1a3df3729b0c9)</sup>

#### main <sup>[link](https://github.com/regolith-linux/regolith-session/tree/main)</sup>
- feat: sway-audio-idle-inhibit as recommends dependency <sup>[link](https://github.com/regolith-linux/regolith-session/commit/1696d3eb056a3dd5d8471b3c7e83968a6d50aa21)</sup>

### regolith-wm-config <sup>[link](https://github.com/regolith-linux/regolith-wm-config)</sup>
#### main <sup>[link](https://github.com/regolith-linux/regolith-wm-config/tree/main)</sup>
- chore!: use seperate Xresource keys for lock program <sup>[link](https://github.com/regolith-linux/regolith-wm-config/commit/fe8b80d05700a8be42071b963cc3b96fc66c6d22)</sup>
- fix: set im-config preference to none <sup>[link](https://github.com/regolith-linux/regolith-wm-config/commit/45f8a76a7901a24307cf107ce815d087408af68b)</sup>
- Add stacking layout to default layout toggle list <sup>[link](https://github.com/regolith-linux/regolith-wm-config/commit/8c51ce1d2fe7cf741438440ff2ac983bec972915)</sup>

### sway-audio-idle-inhibit <sup>[link](https://github.com/regolith-linux/SwayAudioIdleInhibit)</sup>
#### main <sup>[link](https://github.com/regolith-linux/SwayAudioIdleInhibit/tree/main)</sup>
- feat: add systemd to handle process execution and failures <sup>[link](https://github.com/regolith-linux/SwayAudioIdleInhibit/commit/110d61a34d585cccb0065b97221cfc69b1500c1e)</sup>
- Bumped version to 0.2.0 <sup>[link](https://github.com/regolith-linux/SwayAudioIdleInhibit/commit/76cb8d454b1a51bf803906e2587ef51a1efea5d5)</sup>
- Updated build scripts <sup>[link](https://github.com/regolith-linux/SwayAudioIdleInhibit/commit/234f1ab8a597aad523a0113eccc1f9c50c5658db)</sup>
- Use systemd/elogind Inhibit instead of the wayland protocol <sup>[link](https://github.com/regolith-linux/SwayAudioIdleInhibit/commit/e903b3425e8da959761e53b3f2f39ee4712f66a1)</sup>
- Added .clang-format file <sup>[link](https://github.com/regolith-linux/SwayAudioIdleInhibit/commit/3b092d3ff7594ba53a94b762693fdbf48426853b)</sup>

### sway-regolith <sup>[link](https://github.com/regolith-linux/sway-regolith)</sup>
#### packaging/v1.11-regolith <sup>[link](https://github.com/regolith-linux/sway-regolith/tree/packaging/v1.11-regolith)</sup>
- build: bump version to 1.11 <sup>[link](https://github.com/regolith-linux/sway-regolith/commit/990b23c0e584114c3694447344d6bbf44b2d748f)</sup>


### xdg-desktop-portal-regolith <sup>[link](https://github.com/regolith-linux/xdg-desktop-portal-regolith)</sup>
#### debian-bookworm <sup>[link](https://github.com/regolith-linux/xdg-desktop-portal-regolith/tree/debian-bookworm)</sup>
- fix: dark mode not working (#18) <sup>[link](https://github.com/regolith-linux/xdg-desktop-portal-regolith/commit/4cd8c87f2c82fc7f37f54f46e39e8d042beb3184)</sup>

#### main <sup>[link](https://github.com/regolith-linux/xdg-desktop-portal-regolith/tree/main)</sup>
- feat: use none instead of regolith backend for disabling interfaces <sup>[link](https://github.com/regolith-linux/xdg-desktop-portal-regolith/commit/d697c9c551bbfdd83a83845b0e92006b4ca66b38)</sup>
- fix: dark mode not working (#18) <sup>[link](https://github.com/regolith-linux/xdg-desktop-portal-regolith/commit/50aebb53a0d2691d917dfa4bcdf684c63a6b8177)</sup>

#### ubuntu/jammy <sup>[link](https://github.com/regolith-linux/xdg-desktop-portal-regolith/tree/ubuntu/jammy)</sup>
- fix: dark mode not working (#18) <sup>[link](https://github.com/regolith-linux/xdg-desktop-portal-regolith/commit/d0e02f686ec7222cd38b86032616c4de91e07a21)</sup>
