---
Status:
Tags: 
---

(Links: )
## Plugins
- [The Nerd Tree](https://github.com/scrooloose/nerdtree)
The NERDTree is a file system explorer for the Vim editor. It lets you visually explore complex directory hierarchies in the form of a tree (as the name suggests). With The NERDTree, you can quickly open files for reading or editing and perform basic file system operations using your keyboard and mouse. The plugin can be extended with custom mappings using a special API that you can read more about in the plugins’ documentation.
- [Syntastic](https://github.com/vim-syntastic/syntastic)
Syntastic is a syntax checking plugin for Vim that runs files through external syntax checkers then displays any resulting errors to the user. This can be done on demand, or automatically as files are saved. Syntastic has checking plugins for most development languages out there, so it’s worth checking out no matter what language (or languages) you code in.
- [ack.vim](https://www.vim.org/scripts/script.php?script_id=2572)
Many developers opt for ack over grep as the search function for enhanced results. For Vim users, this plugin allows searching with ack from within Vim and shows the results in a split window.
- [ALE - Asynchronous Lint Engine](https://www.vim.org/scripts/script.php?script_id=5449)
ALE (Asynchronous Lint Engine) is a plugin providing linting (syntax checking and semantic errors) in NeoVim 0.2.0+ and Vim 8. It acts as a Vim Language Server Protocol client, and aims to “lint as you type”.

- fzf ❤️ vim – fzf.vim
[fzf](https://github.com/junegunn/fzf) is a general-purpose command-line fuzzy finder, but in itself is not a Vim plugin. The official repository only provides the basic wrapper function for Vim, leaving it up to the users to write their own Vim commands with it. Since this can be a drag, June Gunn created this repository with a bundle of fzf-based commands and mappings extracted from their own .vimrc . These include the “default” implementation of the features users can find in the alternative Vim plugins.

- [vim-colors-solorized](https://github.com/altercation/vim-colors-solarized)
- [lightline](https://github.com/itchyny/lightline.vim)
Lightline is a light and configurable statusline/tabline plugin for Vim. Forked from the deprecated vim-powerline, lightline is a minimalistic plugin that does not depend on other plugins for functionality. At the same time, it lets users configure and customize it to their needs.
- [The NERD Commenter](https://www.vim.org/scripts/script.php?script_id=1218)
It is very hard to understate the importance of comments in efficient and maintainable code. The NERD Commented plugin defines itself as “a Vim plugin for intensely nerdy commenting powers”. The plugin can digest a great variety of different file types and properly comment  each. It can handle single line, multi line, partial line commenting as well as nesting.
- [vim-commentary](https://github.com/tpope/vim-commentary)
This plugin is one of several developed by Tim Pope that have made it into our list. vim-commentary is extremely simple to use: Use gcc to comment out a line (takes a count), gc to comment out the target of a motion (for example, gcap to comment out a paragraph), and gc in visual mode to comment out the selection. That’s it.
-[vim-surround: surround.vim: quoting/parenthesizing made simple](https://github.com/tpope/vim-surround)
https://www.youtube.com/watch?v=Z2Lza11GdSg
The second in our Tim Pope collection is vim-surround. No, it won’t make you able to hear Vim in stereo surround, but will help you manage your parentheses, brackets, quotes, XML tags, and more. The plugin provides mappings to easily delete, change and add such “surroundings” in pairs.
---
Plugin 'Valloric/YouCompleteMe'
Plugin 'scrooloose/syntastic'
Plugin 'bling/vim-airline'
Plugin 'SirVer/ultisnips'
Plugin 'edsono/vim-matchit'
Plugin 'elzr/vim-json'
Plugin 'honza/vim-snippets'
Plugin 'justinmk/vim-sneak'
Plugin 'kien/ctrlp.vim'
Plugin 'ludovicchabant/vim-lawrencium'
Plugin 'majutsushi/tagbar'
Plugin 'mhinz/vim-signify'
Plugin 'plasticboy/vim-markdown'
Plugin 'scrooloose/nerdcommenter'
Plugin 'sjl/gundo. vim'
Plugin 'tpope/vim-fugitive'
Plugin 'tpope/vim-sleuth'
Plugin 'tpope/vim-surround'
Plugin 'tyru/open-browser.vim'
Plugin 'vim-scripts/a.vim'
Color schemes
Plugin 'tomasr/molokai'
Plugin 'flazz/vim-colorschemes'
## New commands
`*`: find all instances of the word where the current cursor is placed
## Neovide
https://neovide.dev/configuration.html
## YouCompleteMe
### Preparation
___
References:
https://medium.com/geekculture/neovim-configuration-for-beginners-b2116dbbde84
https://neovim.io/doc/user/nvim_terminal_emulator.html

Created: 2022-05-07 15:24