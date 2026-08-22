# omarchy-menu-plugin

Fork of the built-in [Omarchy](https://omarchy.org) menu plugin
(`omarchy.menu`) with **Vim navigation** added.

Deploys as the cloned plugin `shafayet.menu`.

## What's different from upstream

One change: in `Menu.qml`, inside the card's `Keys.onPressed` handler:

| Key | Action |
| --- | ------ |
| `Ctrl+J` | Move selection  down |
| `Ctrl+K` | Move selection    up |
| `Ctrl+H` | Move selection  left |
| `Ctrl+L` | Move selection right |

Works in every menu mode — the main command menu, submenus, search results,
and dmenu-style select prompts. Arrow keys and everything else behave exactly
like the stock plugin.

## Requirements

- Omarchy with the Quickshell-based shell (menu era)

## Install

```sh
omarchy plugin add https://github.com/shafayetejaman/omarchy-menu-plugin.git --enable
```

The manifest carries `clonedFrom: omarchy.menu`, so every route that summons the
built-in menu (`Super + A` / `Super + Space`, bar widget, `omarchy menu …`,
dmenu callers) routes to this clone automatically. The built-in stays installed
but inactive while this one is enabled.

## Usage

Open with your usual menu keybind. Start typing to search.

| Key | Action |
| --- | ------ |
| `Up` / `Ctrl+K` | Select previous row |
| `Down` / `Ctrl+J` | Select next row |
| `PageUp` / `PageDown` | Jump 6 rows |
| `Enter` or `Right` or `Ctrl-L` | Activate row / drill into submenu |
| `Backspace` or `Left` or `Ctrl-H` | Go back one level (when filter is empty) |
| `Delete` | Uninstall app (only on an app row, with confirm) |
| `Escape` | Clear filter, then close |

## Configuration

### Menu items (no code changes)

The menu definition is merged from two JSONC files at startup:

- defaults: `/usr/share/omarchy/default/omarchy/omarchy-menu.jsonc`
- yours: `~/.config/omarchy/extensions/omarchy-menu.jsonc`

Your file is merged on top per-key, so you can override a label/icon/action or
add rows without re-declaring everything. Example:

```jsonc
{
  "my.screenshot": {
    "label": "Screenshot to OCR",
    "icon": "󰄀",
    "action": "grim -g \"$(slurp)\" - | tesseract - -"
  }
}
```

Edits are watched and take effect without restarting the shell. Items support
`when:` / `checked:` bash expressions, `provider:` submenus, and aliases — see
the Omarchy manual's shell-plugins page for the full schema.

### Keybindings (code change)

Edit `~/.config/omarchy/plugins/shafayet.menu/Menu.qml` (`Keys.onPressed` in
the `keyCatcher` Item), then run `omarchy-restart-shell` to apply.

## Update / uninstall

```sh
omarchy plugin update shafayet.menu   # pull latest from this repo
omarchy plugin remove shafayet.menu --yes   # built-in comes back
```
