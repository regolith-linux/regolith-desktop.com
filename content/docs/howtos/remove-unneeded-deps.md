---
title: "Remove Unneeded Dependencies"
linkTitle: "Remove Unneeded Dependencies"
description: >
  Remove desktop components unused by Regolith
---

When Regolith is installed via ISO, only the necessary desktop components or packages are installed by default.  However when installing Regolith into an existing Ubuntu system, some desktop components that Regolith does not require will still be installed on the system.  There is nothing wrong with keeping these additional packages, in fact they are important if you wish to switch back to the default Ubuntu session at some point, however some users prefer to remove them.

{{< callout type="error" >}}
Removing packages may cause unexpected behavior or loss of functionality.
{{< /callout >}}

## Install LightDM

The `lightdm` display manager can be installed by with a variety of themes, but we'll install the package `regolith-lightdm-config`:

```bash
sudo apt install regolith-lightdm-config
```

Inspect the output of `apt` to verify you approve of the package changes.  When presented with a menu, choosing between `gdm3` and `lightdm`, select `lightdm`.

## Remove Unneeded Packages

Now that we are using the `lightdm` display manager, we can remove the unneeded packages:

```bash
sudo apt remove gdm3 gnome-shell 'evolution-data-server*'
```

Inspect the output of `apt` to verify you approve of the package changes.

The Regolith sessions need `gnome-session-common`, but on Ubuntu 26.04 no remaining package depends on it, so `apt autoremove` would remove it and logging in would fail. Mark it as manually installed first:

```bash
sudo apt-mark manual gnome-session-common
```

Now we can remove all of the dependencies of those packages:

```bash
sudo apt autoremove
```

If logging in returns you to the login screen after this step, switch to a text console with {{< keys "ctrl,alt,f3" >}}, log in there and run `sudo apt install gnome-session-common`.

## Reboot

At this point the unneeded dependencies have been removed from the system.  Reboot to complete the procedure.

## Undo

If you decide that you'd like to restore the existing setup, run the following and select `gdm3` as the display manager.

```bash
sudo apt install gdm3 gnome-shell evolution-data-server ubuntu-session
sudo apt remove regolith-lightdm-config
```

`lightdm` itself stays installed. Once `gdm3` is the default display manager, you can remove it with `sudo apt remove lightdm`.
