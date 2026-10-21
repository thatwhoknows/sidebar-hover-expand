# Zen Sidebar Hover

A **CSS-only Zen Mod** that keeps Zen's vertical tab sidebar collapsed to a
thin icon strip and expands it smoothly when you hover over it — **pushing**
page content aside instead of covering it.

- No JavaScript. No WebExtension. Pure `chrome.css` loaded by Zen's Mods system.
- Hover in → expand (default `260px`) · Hover out → collapse (default `60px`)
- Content is **pushed**, not overlaid (matches Zen's native expanded layout)
- Works with the right-side sidebar and compact mode
- 5 user preferences, editable in Zen's Mods settings / `about:config`

**Requirements:** Zen Browser ~1.22.x–1.23.x (Firefox 151–158).

## How it works

Zen's vertical tab sidebar is **`#navigator-toolbox`** — a flex sibling of
`#zen-appcontent-wrapper` inside `<hbox id="browser">`. The mod:

1. Pins `#navigator-toolbox` to the collapsed width (`min/max/width !important`,
   overriding Zen's JS-set inline width and CSS variables).
2. Expands it on `#navigator-toolbox:hover`.
3. Content pushes naturally — `#navigator-toolbox` is a flex sibling, so
   widening it reduces the room for `#zen-appcontent-wrapper`. No margin
   hacks needed.
4. Fades labels via `opacity` (no layout thrash).

All rules release when Zen's expand button is used (`zen-sidebar-expanded="true"`
on `:root`), so the native toggle keeps working.

> **Note:** `#sidebar-box` (Firefox's bookmarks/history panel) is a different,
> hidden element in Zen. This mod targets `#navigator-toolbox`, which is what
> actually renders your vertical tabs, workspaces, and essentials.

## Installation

1. Get the `zen-sidebar-hover` folder.
2. Install in Zen: **Settings → Zen Mods** → load/install the mod folder
   (`chrome.css`, `preferences.json`, `icon.png`, `README.md`).
3. Enable the mod.

> `icon.png` in this repo is a generated placeholder (600×400). Replace it
> with final artwork before submitting to the Mods Registry.

## Configuration

Preferences appear under **Settings → Zen Mods → Zen Sidebar Hover** and in
`about:config` as `mod.zen-sidebar-hover.*`. Changes apply **live** — no
reload needed.

| about:config pref | Default | Effect |
|---|---|---|
| `mod.zen-sidebar-hover.sidebar.collapsed-width` | `60px` | Width of the thin icon strip |
| `mod.zen-sidebar-hover.sidebar.expanded-width` | `260px` | Width while hovering |
| `mod.zen-sidebar-hover.sidebar.transition-duration` | `200ms` | Expand/collapse animation speed |
| `mod.zen-sidebar-hover.sidebar.mode` | `push` | `push` shifts page content; `overlay` covers it |
| `mod.zen-sidebar-hover.sidebar.show-labels-on-hover` | `true` | Fade text labels in while hovering |

Use valid CSS units (`px`, `em`, …) for the three string prefs; invalid
values fall back to the built-in defaults (60px / 260px / 200ms).

The `60px` collapsed default matches Zen's own collapsed width
(48px tab + 6px padding × 2). Set it smaller for a tighter strip.

## Compatibility

**Safe to combine with:**

- **Neo Zen** — restyles colors/fonts/spacing; this mod only controls width and label opacity
- **no-gaps** — adjusts tab spacing; this mod doesn't touch spacing
- **icon-tweaker** — adjusts icon sizes; this mod doesn't touch icons
- **context-menu-icons** — adds icons to context menus; this mod doesn't touch context menus
- Other Sine / Zen mods that don't also control `#navigator-toolbox` width

**Potential conflicts:**

- Other sidebar width mods that target `#navigator-toolbox` `min-width`/`max-width`/`width`.
  Disable one of them.

**Zen native features:**

- **Expand button** (`#zen-expand-sidebar-button`): fully supported — clicking it
  sets `zen-sidebar-expanded="true"`, which releases the mod. Click again to
  return to hover behavior.
- **Splitter** (`#zen-sidebar-splitter`): hidden when the mod is active (Zen
  already hides it when collapsed).
- **Compact mode**: the sidebar becomes `position: fixed` (overlays content).
  The mod respects this and only adjusts width + z-index there.
- **Right-side sidebar** (`zen.tabs.vertical.right-side`): fully supported.

## Manual test checklist

1. Collapsed: icons only, ~60px strip, no labels, content flush against it.
2. Hover in: expands to ~260px over 200ms, labels fade in, content **pushes**
   right (no overlap/cutoff).
3. Hover out: collapses, labels fade out, content returns.
4. Set mode to `overlay` in settings: sidebar covers content instead.
5. Toggle compact mode: sidebar overlays content (position: fixed).
6. Enable right-side sidebar: expansion works correctly.
7. Click Zen's expand button: full native expanded sidebar, mod stays out of
   the way; collapse again → hover behavior returns.
8. Change widths/duration in settings: live update, no reload.
9. Install Neo Zen / no-gaps / icon-tweaker: no layout breakage.

## License

MIT
