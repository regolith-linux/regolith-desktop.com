---
title: "Change Workspace Icons and Labels"
linkTitle: "Change Workspace Icons and Labels"
description: >
  Learn how to customize each workspace in the bar
---

Adding icons to workspaces can give your desktop a nice and functional look. The steps on this page work in both the X11 (i3) and the Wayland (sway) session.

## Finding icons

Icons are actually characters from fonts installed by Regolith looks. Every look uses a [Nerd Font](https://www.nerdfonts.com/) for the bar, which contains thousands of icons. The bar font is set by the Xresource `wm.bar.font`; you can see the current one with `xrescat wm.bar.font` in the X11 session or `trawlcat wm.bar.font` in the Wayland session. For example, the default look uses _BitstreamVeraSansMono Nerd Font_.

You can browse the icons on the [Nerd Fonts cheat sheet](https://www.nerdfonts.com/cheat-sheet), or in the font itself using the [GNOME Character Map](https://gitlab.gnome.org/GNOME/gucharmap) application from the `gucharmap` package:

1. Open "Character Map".
2. Select your icon font in the top left combo.
3. In the _script_ pane select _Common_.
4. Scroll down the characters pane until you find the icons.
5. Double click on the icon you are looking to use and press the _Copy_ button (bottom right).

**Tip:** the icons are in the Unicode private use areas, starting at U+E000.

## Changing Icons

Follow these steps to override the default workspace label with your own:

1. Find the character you want to use and copy to the clipboard. This example will use the terminal glyph (U+F120, `nf-fa-terminal` on the cheat sheet): 
2. Add a line to your `Xresources` overrides file with the workspace number and character you wish to use:

{{< callout type="warning" >}}
Regolith version 3.0 onward replaces "i3-wm" with "wm" in Xresource keys.  The content on this page has been updated for Regolith 3.0+.  If you are using an earlier version, use "i3-wm" instead of "wm" in the key names below.  For example `wm.foo.bar` changes to `i3-wm.foo.bar` for Regolith 1.x and 2.x.
{{< /callout >}}

```bash
echo "wm.workspace.06.name: 6:" >> ~/.config/regolith3/Xresources
```

3. Refresh your UI:

```bash
regolith-look refresh
```

Workspace labels are only updated when a workspace is created. Ensure you're creating a new workspace 6 if following the example above, for example with {{< keys "super,6" >}} while workspace 6 does not exist.

## Changing Labels

The same approach can be used to specify textual descriptions of your workspaces. For example, adding the following lines to your `~/.config/regolith3/Xresources` file will change the first three workspaces:

```yaml {filename="~/.config/regolith3/Xresources"}
wm.workspace.01.name: 1: Terminal
wm.workspace.02.name: 2: Web
wm.workspace.03.name: 3: Chat
```

Keep the number and the colon at the start of each name: the workspace keybindings find workspaces by that number, and the bar hides it.

## Further Reading

See the [reference page for configurations]({{< ref "/docs/Reference/configurations.md" >}}) for more details about config files in Regolith.
