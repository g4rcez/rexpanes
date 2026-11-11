# rexpanes

Pick a [Rex](https://www.superlogical.com/rex/docs) pane by its number, like tmux `display-panes`.

Press a key and every pane of the active tab shows a big number. Press a digit to focus that pane.
The focused pane is green and the others are blue.

- `1`–`9` focus that pane. Panes are numbered in reading order: top to bottom, then left to right.
- `Esc`, `q` or any other key closes the picker and keeps the focus where it was.
- A click outside the numbers closes the picker.
- With fewer than 2 panes, nothing happens.

## Requirements

- macOS and [Rex](https://www.superlogical.com/rex/docs)
- `bash` and `jq`

## Install

### Homebrew

```sh
brew install g4rcez/tap/rexpanes
```

This installs the Rex plugin `/opt/homebrew/opt/rexpanes/share/rexpanes/rexpanes.lua` and its script
(use `/usr/local/opt/...` on an Intel Mac). It also installs `jq`.

### From source

Clone this repository and use the path of `rexpanes.lua` in the `dofile` call below.

## Set up

Add this to your Rex `init.lua`:

```lua
dofile("/opt/homebrew/opt/rexpanes/share/rexpanes/rexpanes.lua") {
    key = "ctrl+p>q",
}
```

Rex defines the action `pane_select`. It shows in the action list even without a key.

### The key

`key` is the only option you need. It uses Rex key syntax, and a `>` makes a chord (`ctrl+p>q` is
Ctrl-P, then `q`).

```lua
key = "alt+q"                  -- one key
key = { "ctrl+p>q", "alt+q" }  -- several keys for the same action
```

Without `key`, rexpanes only defines the action. Bind it yourself, in any mode or key table:

```lua
dofile("/opt/homebrew/opt/rexpanes/share/rexpanes/rexpanes.lua") {}
rex.bind("ctrl+p>q", "pane_select")
```

Run `rex keymap` to see what each key does.

### With an AI agent

If your agent uses the Rex skill (`rex skill setup`), ask it to install rexpanes from
[AI_INSTRUCTIONS.md](AI_INSTRUCTIONS.md):

> Install and configure rexpanes. Follow https://raw.githubusercontent.com/g4rcez/rexpanes/main/AI_INSTRUCTIONS.md

## Options

| Option | Default | Meaning |
| --- | --- | --- |
| `key` | none | A key, or a list of keys, bound to `pane_select` |
| `script` | `bin/rexpanes` next to `rexpanes.lua` | The script that draws the numbers |
| `focused` | `9ece6a` | RGB hex of the number of the focused pane |
| `other` | `7aa2f7` | RGB hex of the other numbers |
| `width`, `height` | `0.4`, `0.5` | Size of a float as a fraction of its pane |
| `theme` | TokyoNight | Theme of the floats |

## How it works

`rexpanes.lua` reads the layout of the active tab and opens one small float over each pane. The float of
the focused pane runs `bin/rexpanes pick`, which reads one key. The other floats run `bin/rexpanes show`
and end when the first one ends. Pressing the key again while the picker is open closes it.

## Test

```sh
bash tests/render-test.bash
```

## License

MIT
