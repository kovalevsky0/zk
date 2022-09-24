---
title: "Neovim - LSP Setup"
date: "2022-08-23 14:10:15"
tags:
  - neovim
  - nvim
  - vim
  - linux
public: true
---

# Neovim - LSP Setup

## Install plugin 'lspconfig'

If you use **Packer**

Put it inside vim config file:

```lua
use 'neovim/nvim-lspconfig'
```

## Lua Language Server Configuration

- [github.com/sumneko/lua-language-server](https://github.com/sumneko/lua-language-server)

Install (brew)

```bash
brew install lua-language-server
```

In any lua file or in a specific file that includes LSP configuration:

```lua
require'lspconfig'.sumneko_lua.setup{}
```

- It activates LSP for lua files
- You can check that LSP works for Lua files
  - Open any .lua file
  - type :LspInfo
  - You should see the line `1 client(s) attached to this bufer` and `Client: sumneko_lua`

## JavaScript/TypeScript Server Configuration

- [github.com/typescript-language-server](https://github.com/typescript-language-server/typescript-language-server)

### Install (npm)

```bash
npm install -g typescript-language-server typescript
```

### Install (yarn)

```bash
yarn global add typescript-language-server
```

### nvm

- If you use **Node Version Manager** then **typescript-language-server** will be installed in the specific folder for the specific version of Node
- For example for Node with version **18.7.0** it will be in the path `$NVM_DIR/versions/node/v18.7.0/lib/node_module/typescript-language-server`
- Important!: ensure the server is executable from the command line (just type `typescript-language-server` and it should be available)

## Pre-installing language servers by nvm and npm/yarn

- Some of the language servers are available as npm packages
- They can be installed by npm or yarn
- You can specify the list of language servers that will be installed every time when you install **node** by **nvm**

The list should be in the special config file that is available on the path:

```
$NVM_DIR/default-package
```

An example of file **default-package**:

```
typescript-language-server
pyright

```

There can be any npm packages, not just language servers.

- https://github.com/nvm-sh/nvm#default-global-packages-from-file-while-installing



