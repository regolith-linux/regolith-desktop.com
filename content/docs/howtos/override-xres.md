---
title: "Override Xresources"
linkTitle: "Override Xresources"
description: >
  Learn how to stage user copies of Regolith configuration files
---

Regolith relies on the [Xresources system](https://en.wikipedia.org/wiki/X_resources) to provide a consolidated interface configuration. By changing `Xresources` values, Regolith can be customized in ways such as updating the user interface, specifying custom behaviors, or defining a specific format for some of the blocklets on the bar.

{{< callout type="info" >}}
In Regolith, `Xresources` values can be changed via two methods: supplying your own `Xresources` files, and overriding the existing `Xresources` values. The latter approach is strongly recommended, as it makes upgrading to newer versions of Regolith less likely to break your customizations. If you copy the `Xresources` tree completely you'll need to integrate any breaking changes in future versions manually.
{{< /callout >}}

## Initialization

When a Regolith session is started, Xresources are loaded in the following order. Later files override values from earlier ones:

| File                             | Optional | Notes                                                                                                   |
| -------------------------------- | -------- | ------------------------------------------------------------------------------------------------------- |
| `~/.Xresources`                  | Y        | Intended for non-Regolith settings                                                                      |
| `~/.config/regolith3/Xresources` | Y        | Loaded first so that it can select the look with `regolith.look.path`                                   |
| `/usr/share/regolith-look/<look>/root` | N  | The Regolith `Xresources` associated with the selected look (`regolith-look set`), or with the most recently installed look if none is selected |
| `~/.config/regolith3/Xresources` | Y        | Loaded again, so that your overrides take precedence over the look                                      |

It is recommended to use `~/.config/regolith3/Xresources` for customization as it doesn't require the specification of redundant settings and is easier to maintain over time. It may `#include` files from `~/.config/regolith3/Xresources.d`.

In the X11 session the values are loaded into the X server with `xrdb`. In the Wayland (Sway) session the same files are loaded, in the same order, into the `trawl` configuration daemon instead.

## Determining which values can be changed

The `xrdb` tool can be used to list the existing `Xresources` values in the X11 session. [There is a number of existing overrides]({{< ref "/docs/Reference/xresources.md" >}}):

```console
$ xrdb -query
[...]
gtk.document_font_name:	Sans 11
gtk.font_name:	Bitstream Vera Sans 11
gtk.icon_theme_name:	hicolor
gtk.monospace_font_name:	BitstreamVeraSansMono Nerd Font 12
gtk.theme_name:	Adwaita-dark
wm.bar.background.color:	#393939
[...]
```

In the Wayland session, `xrdb -query` shows nothing. Use `trawldb --query` instead, or `trawlcat <key>` to read a single value:

```console
$ trawldb --query wm.bar.position
wm.bar.position :	bottom
```

## Examples

Note that the commands presented below append text to a file, so running the command more than once will result in duplicate entries.

### Example - Update the UI for High DPI Screens

By using the `~/.config/regolith3/Xresources` override file, we will only need to specify the values we wish to change. The `xrdb` tool can be used to determine what current values are set to.

1. Create or add the following value to your `~/.config/regolith3/Xresources` file:

```yaml {filename="~/.config/regolith3/Xresources"}
Xft.dpi: 192
```

2. Reload the `Xresources` configuration:

```bash
regolith-look refresh
```

3. Open a new terminal to see the change take effect.

`192` is just an example value, please adjust as needed.

`Xft.dpi` applies to the X11 session only. In the Wayland session, scale the output instead, for example with a file in `~/.config/regolith3/sway/config.d/` containing `output * scale 2`.

### Example - Change i3 bar position

```console
$ xrdb -query | grep position     # trawldb --query wm.bar.position in the Wayland session
wm.bar.position:	bottom
$ echo "wm.bar.position:	top" >> ~/.config/regolith3/Xresources
$ regolith-look refresh
```

### Example - Change GTK Theme

```console
$ xrdb -query | grep gtk.theme_name
gtk.theme_name:	Adwaita-dark
$ echo "gtk.theme_name:	Adwaita" >> ~/.config/regolith3/Xresources
$ regolith-look refresh
```

### Example - Disable the System Tray

```bash
echo "wm.bar.trayoutput:	none" >> ~/.config/regolith3/Xresources
regolith-look refresh
```

This setting applies to the X11 session only.

### Example - Use Alt instead of Win as Super

```bash
echo "wm.mod: Mod1" >> ~/.config/regolith3/Xresources
echo "wm.alt: Mod4" >> ~/.config/regolith3/Xresources
echo "wm.carry_container_to_ws_key: Mod4" >> ~/.config/regolith3/Xresources
regolith-look refresh
```

The third line keeps the "Carry Window to Workspace" bindings on {{< keys "super,alt" >}} after the swap. Without it, Regolith 3.4 and earlier give "Duplicate keybinding" errors, because the default for that binding is fixed to `Mod1`. In the Wayland session of those releases, the keyboard layout bindings also use a fixed `Mod1`, so after the swap {{< keys "alt,backspace" >}} switches the keyboard layout instead of toggling the split orientation.

Reloading i3 alone does not read the file again: run `regolith-look refresh` as above, or log back into the session. Some settings may require logging back into the session, for example anything that i3 launches as a separate process.

## Further Reading

See the [reference page for configurations]({{< ref "/docs/Reference/configurations.md" >}}) for more details about config files in Regolith.
