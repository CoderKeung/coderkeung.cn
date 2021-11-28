+++
title = "使用packer做插件管理"
date = 2020-11-10T19:59:03+08:00
draft = false
indent = false
initial = false
toc = false
+++

`vim-plug` 作为 **Vim** 的插件管理器还是很好用的，但是作为一个爱捣鼓的人，最近受 [glepnir](https://github.com/glepnir) 大佬的影响，入了 **Neovim** 的坑，并且使用 Lua 进行配置。这时候 `vim-plug` 就不是很协调，于是乎就转投 `packer` 的怀抱，简单记录，以防遗忘。

在安装之前，你必须确保自己的 **Neovim** 的版本是 **v0.5.0+**

[wbthomason/packer.nvim](https://github.com/wbthomason/packer.nvim)

安装 `packer` 不是很难，使用如下命令直接安装：

```bash
git clone https://github.com/wbthomason/packer.nvim \
 ~/.local/share/nvim/site/pack/packer/opt/packer.nvim
```

安装之后，在 **Neovim** 的配置文件目录之下新建 `lua` 文件夹，并于其下新建 `plugins.lua` 文件，进行 `packer` 配置。

```bash
~/.config/nvim/.
├── init.lua
└── lua
    └── plugins.lua
```

```lua
-- plugins.lua 文件内容
local execute = vim.api.nvim_command
execute('packadd packer.nvim')
local packer = require('packer')
local use = packer.use

return require('packer').startup(function()
	-- Packer can manage itself as an optional plugin
  use {'wbthomason/packer.nvim', opt = true}
end)
```

 随后，在 `init.vim` 或者 `init.lua` 配置文件中启用 `plugins.lua` 文件模块

```lua
-- init.vim
lua require('plugins')

-- init.lua
require('plugins')
```

这样，你就已经安装好 `packer` 了。但是如果你想自动安装的话，需要在 `plugins.lua` 文件头部中添加这样几行：

```lua
local execute = vim.api.nvim_command
local fn = vim.fn

local install_path = fn.stdpath('data')..'/site/pack/packer/opt/packer.nvim'

if fn.empty(fn.glob(install_path)) > 0 then
  execute('!git clone https://github.com/wbthomason/packer.nvim '..install_path)
end
```

如此以来，就实现了自动识别于安装。
