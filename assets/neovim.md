# Neovim in VSCode

Easy Neovim usage in VSCode.

## Installing

VSCode plugin: [VSCode Neovim](https://marketplace.visualstudio.com/items?itemName=asvetliakov.vscode-neovim)

Installing Neovim:

```powershell
winget install Neovim.Neovim
```

## Configuration

On Windows:

```bash
# Create nvim directories
mkdir ~/AppData/Local/nvim && cd $_
# Installing vim-plug
mkdir autoload plugged
sh -c 'curl -fLo "~/AppData/Local/nvim/autoload/plug.vim --create-dirs \
       https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
# Edit nvim config
nvim init.vim
```

Then editing init.vim:

```plaintext
"vim-plug config
call plug#begin('~/AppData/Local/nvim/plugged')

Plug 'justinmk/vim-sneak'
Plug 'tpope/vim-surround'

call plug#end()

"Make yank in vim sync to system clipboard
"set clipboard=unnamedplus

"Ignore case sensitive when search
:set ignorecase
:set smartcase

"Load user custom keymaps
lua require "user.keymaps"

"Make vim-sneak list labeled for quicking jump
let g:sneak#label = 1

```

Save & exit `init.vim`, then open any file by nvim, run:

```plaintext
:PlugInstall
```

After plugins installed, run:

```plaintext
mkdir -p lua/user && cd $_
nvim keymaps.lua
```

In keymaps.lua:

```plaintext
--print("keymap config")
local opts = { noremap = true, silent = true }
local map = vim.api.nvim_set_keymap

--print("define leader key, default is \, here is space")
vim.g.mapleader = ' '
vim.g.maplocalleader = ' '

--print("keymap list")
map("n", "<leader>y", '"*y', opts)
map("n", "<leader>p", '"*p', opts)
map("n", "<leader>P", '"*P', opts)


```

That's all, open your VSCode and enjoy it.

## References

[你真的会装 vim 插件吗？【vim-plug】详解](https://www.bilibili.com/read/cv8264341)
