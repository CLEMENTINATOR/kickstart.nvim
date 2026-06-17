# nvim

My personal Neovim configuration. Originally based on
[kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim), now restructured
into a modular layout and managed with the built-in `vim.pack` plugin manager.

## Requirements

- Neovim (latest stable or nightly)
- `git`, a C compiler (`gcc`), `make`, `unzip`
- [ripgrep](https://github.com/BurntSushi/ripgrep) and
  [fd](https://github.com/sharkdp/fd) (used by snacks pickers)
- A [Nerd Font](https://www.nerdfonts.com/) (`vim.g.have_nerd_font` is set)
- A clipboard tool (falls back to OSC52 over SSH)
- Per-language toolchains as needed: `npm`/`node` (TS), `go` (gopls),
  `uv` (pyright), `clangd`, `stylua`, `prettier`, etc.

## Install

```sh
git clone <this-repo> "${XDG_CONFIG_HOME:-$HOME/.config}/nvim"
nvim
```

On first launch `vim.pack` downloads all plugins automatically. Treesitter
parsers update via the `PackChanged` autocmd in `init.lua`.

## Layout

```
init.lua             entrypoint: leader, requires, vim.pack plugin list, Pack* commands
filetype.lua         filetype overrides
lua/
  options.lua        vim options, clipboard/OSC52 setup, folding
  keymaps.lua        global keymaps
  autocmds.lua       autocommands (yank highlight, resize, session)
  commands.lua       user commands
  diagnostics.lua    diagnostic config + toggles
  session.lua        automatic per-directory sessions
  term.lua           terminal helpers
  icons.lua          shared icons
  utils/             shared helpers (utils, log)
plugin/              one file per plugin/feature, auto-sourced by Neovim
```

## Plugin management

Plugins are declared with `vim.pack.add` in `init.lua` and each `plugin/*.lua`
file. Helper commands defined in `init.lua`:

| Command       | Action                                              |
| :------------ | :-------------------------------------------------- |
| `:PackUpdate` | Update plugins                                      |
| `:PackSync`   | Update plugins to versions pinned in the lockfile   |
| `:PackStatus` | Show plugin status (offline)                        |
| `:PackClean`  | Remove orphaned (inactive) plugins                  |

The lockfile is `nvim-pack-lock.json`.
