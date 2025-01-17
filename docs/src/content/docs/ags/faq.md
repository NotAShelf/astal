---
title: FAQ
description: Frequently asked question, common issues, tips and tricks
---

## Monitor id does not match compositor

The monitor property that windows expect is mapped by Gdk, which is not always
the same as the compositor. Instead use the `gdkmonitor` property which expects
a `Gdk.Monitor` object which you can get from compositor libraries.

Example with Hyprland

```js
import Hyprland from "gi://AstalHyprland"

function Bar(gdkmonitor) {
    return <window gdkmonitor={gdkmonitor} />
}

function main() {
    for (const m of Hyprland.get_default().get_monitors()) {
        Bar(m.gdk_monitor)
    }
}

App.start({ main })
```

## Environment variables

JavaScript is **not** an bash.

```tsx
const HOME = exec("echo $HOME") // does not work
```

`exec` and `execAsync` runs the passed program as is, its **not** run in a
shell environment, so the above example just passes `$HOME` as a string literal
to the `echo` program.

:::caution[Please don't do this]
You could pass it to bash, but that is a horrible approach.

```tsx
const HOME = exec("bash -c 'echo $HOME'")
```

:::

You can read environment variables with [GLib.getenv](https://gjs-docs.gnome.org/glib20~2.0/glib.getenv).

```tsx
import GLib from "gi://GLib"

const HOME = GLib.getenv("HOME")
```

## Custom svg symbolic icons

Put the svgs in a directory, named `<icon-name>-symbolic.svg`
and use `App.add_icons` or `icons` parameter in `App.start`

```js
// app.ts
App.start({
    icons: `${SRC}/icons`,
    main() {
        Widget.Icon({
            icon: "custom-symbolic", // custom-symbolic.svg
            css: "color: green;", // can be colored, like other named icons
        })
    },
})
```

:::note
If there is a name clash with an icon from your current icon pack
the icon pack will take precedence
:::
