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

### Changes in `debian bookworm` since `release-3_3`
#### [arc-icon-theme](https://github.com/regolith-linux/arc-icon-theme)
##### [v20250203-1...v20251012-1](https://github.com/regolith-linux/arc-icon-theme/compare/v20250203-1...v20251012-1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/arc-icon-theme/commit/392d7cc2f849e48afb414c1f9d5539d0a542ec7a)

#### [ayu-theme](https://github.com/regolith-linux/ayu-theme)
##### [v0.2.3-1...v0.2.4-1](https://github.com/regolith-linux/ayu-theme/compare/v0.2.3-1...v0.2.4-1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/ayu-theme/commit/e23b62dacb0947e9df59825c6872a2a7b2b6286f)

#### [dracula-gtk](https://github.com/regolith-linux/dracula-gtk)
##### [v1.1.2...v1.1.3](https://github.com/regolith-linux/dracula-gtk/compare/v1.1.2...v1.1.3)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/dracula-gtk/commit/f8c03d408900cd3e295a561ebd992b126a9bfb80)

#### [fonts-jetbrains-mono](https://github.com/regolith-linux/fonts-jetbrains-mono)
##### [v1.0.2-1regolith1...v1.0.3-1regolith1](https://github.com/regolith-linux/fonts-jetbrains-mono/compare/v1.0.2-1regolith1...v1.0.3-1regolith1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-jetbrains-mono/commit/e8350095b4a1563f872d2b8e9f74faa553b9b61a)

#### [fonts-materialdesignicons-webfont](https://github.com/regolith-linux/fonts-materialdesignicons-webfont)
##### [v1.6.50-4regolith1...v1.6.50-5regolith1](https://github.com/regolith-linux/fonts-materialdesignicons-webfont/compare/v1.6.50-4regolith1...v1.6.50-5regolith1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-materialdesignicons-webfont/commit/3dbf43ce93981dfaeae6e8d4f9f78598d649c0e0)

#### [fonts-source-code-pro-ttf](https://github.com/regolith-linux/fonts-source-code-pro-ttf)
##### [v1.011-0ubuntu1-ppa2...v1.011-0ubuntu1-ppa3](https://github.com/regolith-linux/fonts-source-code-pro-ttf/compare/v1.011-0ubuntu1-ppa2...v1.011-0ubuntu1-ppa3)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-source-code-pro-ttf/commit/5db0bb1635cb63570c222554111d52ddd7680bc4)

#### [gruvbox-gtk](https://github.com/regolith-linux/gruvbox-gtk)
##### [v1.0.2-1...v1.0.2-2](https://github.com/regolith-linux/gruvbox-gtk/compare/v1.0.2-1...v1.0.2-2)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/gruvbox-gtk/commit/edb2f30e754738804fcbd568d6fa2577a471ad0f)

#### [ilia](https://github.com/regolith-linux/ilia)
##### [v0.16.0...v0.17.0](https://github.com/regolith-linux/ilia/compare/v0.16.0...v0.17.0)
- [Load icons async](https://github.com/regolith-linux/ilia/commit/ecbdf9d151d70e98feaca8c9ab8b111ab6205fc2)
- [Fix prefix matching to sort correct](https://github.com/regolith-linux/ilia/commit/7385909af3df86510ebb61a883ace4e9bf2d82b1)
- [Extract app sorting into a dedicated function for testing](https://github.com/regolith-linux/ilia/commit/20d2030b5a79c170ce4c6bc43ecd5fad0a83f8d0)
- [Respect icon size=0 harder](https://github.com/regolith-linux/ilia/commit/687835a9a1769318efb348140e58ea6dccc683c7)
- [Removed newlines in debug() statements](https://github.com/regolith-linux/ilia/commit/d5782f569f54a05146776e926a09250cdcb7f9d3)


### Changes in `ubuntu jammy` since `release-3_3`
#### [arc-icon-theme](https://github.com/regolith-linux/arc-icon-theme)
##### [v20250203-1...v20251012-1](https://github.com/regolith-linux/arc-icon-theme/compare/v20250203-1...v20251012-1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/arc-icon-theme/commit/392d7cc2f849e48afb414c1f9d5539d0a542ec7a)

#### [armesto](https://github.com/regolith-linux/armesto)
##### [v0.2.1ubuntu1...v0.2.2ubuntu1](https://github.com/regolith-linux/armesto/compare/v0.2.1ubuntu1...v0.2.2ubuntu1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/armesto/commit/348a3d0b38d9c140c9dbfc7c7fa6157806ef71df)

#### [ayu-theme](https://github.com/regolith-linux/ayu-theme)
##### [v0.2.3-1...v0.2.4-1](https://github.com/regolith-linux/ayu-theme/compare/v0.2.3-1...v0.2.4-1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/ayu-theme/commit/e23b62dacb0947e9df59825c6872a2a7b2b6286f)

#### [dracula-gtk](https://github.com/regolith-linux/dracula-gtk)
##### [v1.1.2...v1.1.3](https://github.com/regolith-linux/dracula-gtk/compare/v1.1.2...v1.1.3)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/dracula-gtk/commit/f8c03d408900cd3e295a561ebd992b126a9bfb80)

#### [fonts-jetbrains-mono](https://github.com/regolith-linux/fonts-jetbrains-mono)
##### [v1.0.2-1regolith1...v1.0.3-1regolith1](https://github.com/regolith-linux/fonts-jetbrains-mono/compare/v1.0.2-1regolith1...v1.0.3-1regolith1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-jetbrains-mono/commit/e8350095b4a1563f872d2b8e9f74faa553b9b61a)

#### [fonts-materialdesignicons-webfont](https://github.com/regolith-linux/fonts-materialdesignicons-webfont)
##### [v1.6.50-4regolith1...v1.6.50-5regolith1](https://github.com/regolith-linux/fonts-materialdesignicons-webfont/compare/v1.6.50-4regolith1...v1.6.50-5regolith1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-materialdesignicons-webfont/commit/3dbf43ce93981dfaeae6e8d4f9f78598d649c0e0)

#### [fonts-source-code-pro-ttf](https://github.com/regolith-linux/fonts-source-code-pro-ttf)
##### [v1.011-0ubuntu1-ppa2...v1.011-0ubuntu1-ppa3](https://github.com/regolith-linux/fonts-source-code-pro-ttf/compare/v1.011-0ubuntu1-ppa2...v1.011-0ubuntu1-ppa3)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-source-code-pro-ttf/commit/5db0bb1635cb63570c222554111d52ddd7680bc4)

#### [gruvbox-gtk](https://github.com/regolith-linux/gruvbox-gtk)
##### [v1.0.2-1...v1.0.2-2](https://github.com/regolith-linux/gruvbox-gtk/compare/v1.0.2-1...v1.0.2-2)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/gruvbox-gtk/commit/edb2f30e754738804fcbd568d6fa2577a471ad0f)

#### [ilia](https://github.com/regolith-linux/ilia)
##### [v0.16.0...v0.17.0](https://github.com/regolith-linux/ilia/compare/v0.16.0...v0.17.0)
- [Load icons async](https://github.com/regolith-linux/ilia/commit/ecbdf9d151d70e98feaca8c9ab8b111ab6205fc2)
- [Fix prefix matching to sort correct](https://github.com/regolith-linux/ilia/commit/7385909af3df86510ebb61a883ace4e9bf2d82b1)
- [Extract app sorting into a dedicated function for testing](https://github.com/regolith-linux/ilia/commit/20d2030b5a79c170ce4c6bc43ecd5fad0a83f8d0)
- [Respect icon size=0 harder](https://github.com/regolith-linux/ilia/commit/687835a9a1769318efb348140e58ea6dccc683c7)
- [Removed newlines in debug() statements](https://github.com/regolith-linux/ilia/commit/d5782f569f54a05146776e926a09250cdcb7f9d3)


### Changes in `ubuntu noble` since `release-3_3`
#### [arc-icon-theme](https://github.com/regolith-linux/arc-icon-theme)
##### [v20250203-1...v20251012-1](https://github.com/regolith-linux/arc-icon-theme/compare/v20250203-1...v20251012-1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/arc-icon-theme/commit/392d7cc2f849e48afb414c1f9d5539d0a542ec7a)

#### [armesto](https://github.com/regolith-linux/armesto)
##### [v0.2.1ubuntu1...v0.2.2ubuntu1](https://github.com/regolith-linux/armesto/compare/v0.2.1ubuntu1...v0.2.2ubuntu1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/armesto/commit/348a3d0b38d9c140c9dbfc7c7fa6157806ef71df)

#### [ayu-theme](https://github.com/regolith-linux/ayu-theme)
##### [v0.2.3-1...v0.2.4-1](https://github.com/regolith-linux/ayu-theme/compare/v0.2.3-1...v0.2.4-1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/ayu-theme/commit/e23b62dacb0947e9df59825c6872a2a7b2b6286f)

#### [childe](https://github.com/regolith-linux/childe)
##### [v0.1.3...v0.1.4](https://github.com/regolith-linux/childe/compare/v0.1.3...v0.1.4)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/childe/commit/b7f47426c3139e61affeb04fbbe472cd503192d2)

#### [dracula-gtk](https://github.com/regolith-linux/dracula-gtk)
##### [v1.1.2...v1.1.3](https://github.com/regolith-linux/dracula-gtk/compare/v1.1.2...v1.1.3)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/dracula-gtk/commit/f8c03d408900cd3e295a561ebd992b126a9bfb80)

#### [fonts-jetbrains-mono](https://github.com/regolith-linux/fonts-jetbrains-mono)
##### [v1.0.2-1regolith1...v1.0.3-1regolith1](https://github.com/regolith-linux/fonts-jetbrains-mono/compare/v1.0.2-1regolith1...v1.0.3-1regolith1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-jetbrains-mono/commit/e8350095b4a1563f872d2b8e9f74faa553b9b61a)

#### [fonts-materialdesignicons-webfont](https://github.com/regolith-linux/fonts-materialdesignicons-webfont)
##### [v1.6.50-4regolith1...v1.6.50-5regolith1](https://github.com/regolith-linux/fonts-materialdesignicons-webfont/compare/v1.6.50-4regolith1...v1.6.50-5regolith1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-materialdesignicons-webfont/commit/3dbf43ce93981dfaeae6e8d4f9f78598d649c0e0)

#### [fonts-source-code-pro-ttf](https://github.com/regolith-linux/fonts-source-code-pro-ttf)
##### [v1.011-0ubuntu1-ppa2...v1.011-0ubuntu1-ppa3](https://github.com/regolith-linux/fonts-source-code-pro-ttf/compare/v1.011-0ubuntu1-ppa2...v1.011-0ubuntu1-ppa3)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-source-code-pro-ttf/commit/5db0bb1635cb63570c222554111d52ddd7680bc4)

#### [gruvbox-gtk](https://github.com/regolith-linux/gruvbox-gtk)
##### [v1.0.2-1...v1.0.2-2](https://github.com/regolith-linux/gruvbox-gtk/compare/v1.0.2-1...v1.0.2-2)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/gruvbox-gtk/commit/edb2f30e754738804fcbd568d6fa2577a471ad0f)

#### [ilia](https://github.com/regolith-linux/ilia)
##### [v0.16.0...v0.17.0](https://github.com/regolith-linux/ilia/compare/v0.16.0...v0.17.0)
- [Load icons async](https://github.com/regolith-linux/ilia/commit/ecbdf9d151d70e98feaca8c9ab8b111ab6205fc2)
- [Fix prefix matching to sort correct](https://github.com/regolith-linux/ilia/commit/7385909af3df86510ebb61a883ace4e9bf2d82b1)
- [Extract app sorting into a dedicated function for testing](https://github.com/regolith-linux/ilia/commit/20d2030b5a79c170ce4c6bc43ecd5fad0a83f8d0)
- [Respect icon size=0 harder](https://github.com/regolith-linux/ilia/commit/687835a9a1769318efb348140e58ea6dccc683c7)
- [Removed newlines in debug() statements](https://github.com/regolith-linux/ilia/commit/d5782f569f54a05146776e926a09250cdcb7f9d3)


### Changes in `ubuntu plucky` since `release-3_3`
#### [arc-icon-theme](https://github.com/regolith-linux/arc-icon-theme)
##### [v20250203-1...v20251012-1](https://github.com/regolith-linux/arc-icon-theme/compare/v20250203-1...v20251012-1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/arc-icon-theme/commit/392d7cc2f849e48afb414c1f9d5539d0a542ec7a)

#### [armesto](https://github.com/regolith-linux/armesto)
##### [v0.2.1ubuntu1...v0.2.2ubuntu1](https://github.com/regolith-linux/armesto/compare/v0.2.1ubuntu1...v0.2.2ubuntu1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/armesto/commit/348a3d0b38d9c140c9dbfc7c7fa6157806ef71df)

#### [ayu-theme](https://github.com/regolith-linux/ayu-theme)
##### [v0.2.3-1...v0.2.4-1](https://github.com/regolith-linux/ayu-theme/compare/v0.2.3-1...v0.2.4-1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/ayu-theme/commit/e23b62dacb0947e9df59825c6872a2a7b2b6286f)

#### [childe](https://github.com/regolith-linux/childe)
##### [v0.1.3...v0.1.4](https://github.com/regolith-linux/childe/compare/v0.1.3...v0.1.4)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/childe/commit/b7f47426c3139e61affeb04fbbe472cd503192d2)

#### [dracula-gtk](https://github.com/regolith-linux/dracula-gtk)
##### [v1.1.2...v1.1.3](https://github.com/regolith-linux/dracula-gtk/compare/v1.1.2...v1.1.3)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/dracula-gtk/commit/f8c03d408900cd3e295a561ebd992b126a9bfb80)

#### [fonts-jetbrains-mono](https://github.com/regolith-linux/fonts-jetbrains-mono)
##### [v1.0.2-1regolith1...v1.0.3-1regolith1](https://github.com/regolith-linux/fonts-jetbrains-mono/compare/v1.0.2-1regolith1...v1.0.3-1regolith1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-jetbrains-mono/commit/e8350095b4a1563f872d2b8e9f74faa553b9b61a)

#### [fonts-materialdesignicons-webfont](https://github.com/regolith-linux/fonts-materialdesignicons-webfont)
##### [v1.6.50-4regolith1...v1.6.50-5regolith1](https://github.com/regolith-linux/fonts-materialdesignicons-webfont/compare/v1.6.50-4regolith1...v1.6.50-5regolith1)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-materialdesignicons-webfont/commit/3dbf43ce93981dfaeae6e8d4f9f78598d649c0e0)

#### [fonts-source-code-pro-ttf](https://github.com/regolith-linux/fonts-source-code-pro-ttf)
##### [v1.011-0ubuntu1-ppa2...v1.011-0ubuntu1-ppa3](https://github.com/regolith-linux/fonts-source-code-pro-ttf/compare/v1.011-0ubuntu1-ppa2...v1.011-0ubuntu1-ppa3)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/fonts-source-code-pro-ttf/commit/5db0bb1635cb63570c222554111d52ddd7680bc4)

#### [gruvbox-gtk](https://github.com/regolith-linux/gruvbox-gtk)
##### [v1.0.2-1...v1.0.2-2](https://github.com/regolith-linux/gruvbox-gtk/compare/v1.0.2-1...v1.0.2-2)
- [feat: use build-only to test pull request](https://github.com/regolith-linux/gruvbox-gtk/commit/edb2f30e754738804fcbd568d6fa2577a471ad0f)

#### [ilia](https://github.com/regolith-linux/ilia)
##### [v0.16.0...v0.17.0](https://github.com/regolith-linux/ilia/compare/v0.16.0...v0.17.0)
- [Load icons async](https://github.com/regolith-linux/ilia/commit/ecbdf9d151d70e98feaca8c9ab8b111ab6205fc2)
- [Fix prefix matching to sort correct](https://github.com/regolith-linux/ilia/commit/7385909af3df86510ebb61a883ace4e9bf2d82b1)
- [Extract app sorting into a dedicated function for testing](https://github.com/regolith-linux/ilia/commit/20d2030b5a79c170ce4c6bc43ecd5fad0a83f8d0)
- [Respect icon size=0 harder](https://github.com/regolith-linux/ilia/commit/687835a9a1769318efb348140e58ea6dccc683c7)
- [Removed newlines in debug() statements](https://github.com/regolith-linux/ilia/commit/d5782f569f54a05146776e926a09250cdcb7f9d3)


