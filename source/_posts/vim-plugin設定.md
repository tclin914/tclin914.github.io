---
title: vim plugin設定
categories: vim
tags: vim
abbrlink: 14a4a927
date: 2017-02-05 19:10:36
updated: 2017-02-05 19:10:36
---
1. 先將Vundle抓下來
```
    $ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```
2. 下面內容複製到~/.vimrc
```vim
    set nocompatible
    filetype off

    " NERDTree Key
    map <F1> :NERDTree <CR>
    let g:NERDTreeWinPos = 'right'

    set rtp+=~/.vim/bundle/Vundle.vim 
    call vundle#begin()

    " Let Vundle manage itself
    Plugin 'VundleVim/Vundle.vim'

    " Better file browser
    Plugin 'scrooloose/nerdtree'
    " Code commenter
    Plugin 'scrooloose/nerdcommenter'
    " Surround
    Plugin 'tpope/vim-surround'
    " Autoclose
    Plugin 'Townk/vim-autoclose'
    " Airline
    Plugin 'bling/vim-airline'
    " Terminal Vim with 256 colors colorscheme
    Plugin 'fisadev/fisa-vim-colorscheme'
    " Git integration
    Plugin 'motemen/git-vim'

    " Plugin 'Valloric/YouCompleteMe'

    " " SnipMate
    Plugin 'MarcWeber/vim-addon-mw-utils'
    Plugin 'tomtom/tlib_vim'
    Plugin 'garbas/vim-snipmate'
    Plugin 'honza/vim-snippets'

    " markdown
    Plugin 'plasticboy/vim-markdown'

    " "Python and other languages code checker
    Plugin 'scrooloose/syntastic'

    " Rust
    Plugin 'rust-lang/rust.vim'

    call vundle#end()

    " NERDTree Key
    map <F1> :NERDTree <CR>
    let g:NERDTreeWinPos = 'right'

    " NERDcommenter
    let g:NERDSpaceDelims = 1

    " Airline
    set laststatus=2
    let g:airline#extensions#tabline#enabled = 1
    let g:airline#extensions#tabline#left_sep = '  '
    let g:airline#extensions#tabline#left_alt_sep = '|'
    let g:airline#extensions#tabline#right_sep = '  '
    let g:airline#extensions#tabline#buffer_nr_show = 1
    let g:airline_powerline_fonts = 1
    " let g:airline_theme = 'luna'
    let g:airline#extensions#whitespace#enabled = 0

    if !exists('g:airline_symbols')
      let g:airline_symbols = {}
    endif
    let g:airline_left_sep = ''
    let g:airline_left_alt_sep = ''
    let g:airline_right_sep = ''
    let g:airline_right_alt_sep = '|'
    let g:airline_symbols.branch = ''
    let g:airline_symbols.readonly = '|'
    let g:airline_symbols.linenr = '|'

    let g:ycm_global_ycm_extra_conf = '~/.ycm_extra_conf.py'
    let g:ycm_confirm_extra_conf = 0
    let g:ycm_key_list_select_completion=[]
    let g:ycm_key_list_previous_completion=[]

    " Disable markdown folding
    let g:vim_markdown_folding_disabled = 1

    " allow plugins by file type (required for plugins!)
    filetype plugin on
    filetype indent on

    autocmd BufRead,BufNewFile *.ll set filetype=llvm

    set expandtab
    set tabstop=4
    set softtabstop=4
    set shiftwidth=4

    set incsearch
    set hlsearch

    set backspace=2
    set autoindent
    set ruler
    set showmode
    set nu
    set bg=dark
    syntax on

    set clipboard=unnamed

    colorscheme default
```

3. 安裝Plugin

    開啟vim
```
        $ vim
```

    執行安裝動作
```
        :PluginInstall
```
