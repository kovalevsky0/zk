---
title: Neovim
date: "2021-09-06 00:09"
tags:
  - neovim
  - vim
public: true
---

# Neovim

* [vim](vim.md)-based text-editor
* Since 0.5 version it is configurable by using [Lua](Lua.md) programming language

## Links

* [neovim.io](https://neovim.io)
* [GitHub](https://github.com/neovim/neovim)

## Configuring

### plugins

* interesting
  * [formatter.nvim](https://github.com/mhartington/formatter.nvim)
* [hologram.nvim](https://github.com/edluffy/hologram.nvim)

### colorschemes

* [tokyonight.nvim](https://github.com/folke/tokyonight.nvim)

### Complete configurations

- [MiaadTeam/lesvim](https://github.com/MiaadTeam/lesvim)

## Tips and tricks

### LSP Rename symbol

- When you rename some symbol in the code that is used in many files, nvim will change the name of the symbol in files
  - The files will be modified but not saved
- You can save all files at once to apply name changing:

```
:writeAll
```

or

```
:wa
```


