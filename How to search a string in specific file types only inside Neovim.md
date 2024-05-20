# How to search a string in specific file types only inside Neovim

- What you need:
  - [Neovim](https://github.com/neovim/neovim)
  - [LazyVim](https://github.com/LazyVim/LazyVim)
  - [lazy.nvim](https://github.com/folke/lazy.nvim)
  - [telescope.nvim](https://github.com/nvim-telescope/telescope.nvim)
  - [telescope-live-grep-args.nvim](https://github.com/nvim-telescope/telescope-live-grep-args.nvim)

## Install `telescope-live-grep-args.nvim`

In `LazyVim` distrubution

Create configuration file for `telescope.nvim` in path

```
~/.config/nvim/lua/plugins/telescope.lua
```

with following code:

```lua
return {
  "nvim-telescope/telescope.nvim",
  dependencies = {
    {
      "nvim-telescope/telescope-live-grep-args.nvim",
      -- This will not install any breaking changes.
      -- For major updates, this must be adjusted manually.
      version = "^1.0.0",
    },
  },
  config = function()
    require("telescope").load_extension("live_grep_args")
  end,
}
```
Restart `neovim`

## Searching using telescope grep with arguments

- Open telescope grep with args in neovim using command: `:Telescope live_grep_args`
- Type `"<YOUR_STRING>" -g *.<EXTENSION>`

![Screenshot]("./screenshoft-2024-05-20-14-14-55.png")

