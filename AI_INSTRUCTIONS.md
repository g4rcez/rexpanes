# AI instructions: install and configure rexpanes

This file is for AI agents that use the Rex skill (`/rex`) and the `rex` command. Follow the steps in
order. When all checks pass, rexpanes is installed and bound to a key.

rexpanes is a Rex plugin: a Lua file that `init.lua` loads (`rexpanes.lua`), and a bash script it runs
in small floats (`bin/rexpanes`). Both are installed by Homebrew.

## Rules

- Work only on macOS.
- Change only what this file says. Do not edit other keys, modes or actions in `init.lua`.
- Do not run `rexpanes` in your own terminal. It reads `/dev/tty`. Only the Lua plugin starts it.
- At the end, tell the user the installed package, the lines you added to `init.lua`, and the key you chose.

## 1. Install

```sh
brew install g4rcez/tap/rexpanes
REXPANES_LUA="$(brew --prefix)/opt/rexpanes/share/rexpanes/rexpanes.lua"
test -f "$REXPANES_LUA" && test -x "$(dirname "$REXPANES_LUA")/bin/rexpanes" && echo "$REXPANES_LUA"
```

The command must print the path. To upgrade, run `brew upgrade g4rcez/tap/rexpanes`.

## 2. Choose the key

Ask the user for a key, or use `ctrl+p>q` (the chord `Ctrl-P`, then `q`). Run `rex keymap` and make sure
the key is not used. `key` can be one string or a list of strings.

## 3. Edit `init.lua`

Rex reads `$REX_CONFIG` if set, else `$XDG_CONFIG_HOME/rex/init.lua`, else `~/.config/rex/init.lua`.
Add one call, with the path from step 1:

```lua
dofile("/opt/homebrew/opt/rexpanes/share/rexpanes/rexpanes.lua") {
    key = "ctrl+p>q",
}
```

## 4. Check

```sh
rex config check
rex config reload
rex keymap | grep pane_select
```

`rex keymap` must show the key you chose. Do not press the key in your own pane: the numbers cover the
panes and wait for a key.

## Troubleshooting

- `rex config check` reports an error in `rexpanes.lua`: the `dofile` path is wrong. Use the path from step 1.
- The key does nothing: another binding uses it. Choose another key.
- No numbers on the panes: the tab has fewer than 2 panes. The picker needs at least 2.
- The floats close at once: `jq` or `rex` is not on the `PATH` of the floats. `brew install jq`.
