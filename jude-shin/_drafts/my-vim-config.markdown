---
layout: post
title:  "My Vim Config"
date:   2025-08-11 22:31:36 -0700
categories: vim config
---
## Intro
I have my vim config listed on my my github [.dotfiles][github-dotfiles-repo] repo. The 

## Modularity 
I like to have my .config files neat and organized. If I just want to mess with the vim-plug packages, then I want to make sure that I edit that code alone, while not accidentally breaking something else. Modularity in my vim config allows for this. Note: there is another layer of modularity that I have with my .dotfiles, but I will save it for [a different article][explaining-my-.dotfiles-repo]

I break down my vim config file into different parts. For example, I personally break it into the following: 
{% highlight vimscript %}
runtime! archlinux.vim

runtime ./modules/general.vim
runtime ./modules/keybinds.vim
runtime ./modules/filetype.vim
runtime ./modules/plugins.vim
runtime ./modules/colorscheme.vim
{% endhighlight %}

## General 
## Plugins 
I used to install plugins throught the operating system that I used. Arch linux's AUR had a good list of binaries, but then I switched over to using vim-plug. It is cleaner and more portable than using the AUR. 
To install plugins, add the '''plug <plugin-name>''' to the header. Here is my plugins.vim file:

{% highlight vimscript %}
" vim-plug setup
call plug#begin('~/.config/vim/plugged/')
" launches localhost, and opens your browser to view markdown files. Nice for taking notes.
Plug 'iamcco/markdown-preview.nvim', { 'do': 'cd app && npx --yes yarn install' }

" Conquerer of Completion
Plug 'neoclide/coc.nvim', {'branch': 'release'}

" Completion
Plug 'Olical/conjure' " 

" a nice colorscheme that matches the terminal colorscheme that I like
Plug 'morhetz/gruvbox'
call plug#end()

" ...

{% endhighlight %}

I also add plugin specific settings in this file so I always know where to look when a plugin doesn't work.

## Filetypes
For certain languages, I want to work with certain linting styles, or particular vim-motions that are useful. For example, for large text or markdown files where long sentences will wrapt the screen, I would like the cursor to travel up when I hit 'k', even when it is still considered the same line number. For this, I add the collowing lines of code to my filetype.vim script.

{% highlight vimscript %}
augroup md 
	autocmd! 

    " turn on spelling 
	autocmd FileType text,markdown setlocal spell

    " allow for movement within lines that wrap the screen
	autocmd FileType text,markdown map j gj
	autocmd FileType text,markdown map k gk
augroup END
{% endhighlight %}

All filetype rules follow the same structure: checking the filetype first, then executing some commands within that '''autocmd''' block. The '''augroup''' name is just a formality. 

## Keybinds

{% highlight vimscript %}
" time between the remaps
" (like the time between 'k' and 'j' when I am trying to escape)
set timeoutlen=400

" ESCAPE REMAP
inoremap kj <esc>
vnoremap kj <esc>
cnoremap kj <C-C>

BRACKET COMPLETION
inoremap { {}<Esc>ha
inoremap ( ()<Esc>ha
inoremap [ []<Esc>ha
inoremap " ""<Esc>ha
inoremap ' ''<Esc>ha
inoremap ` ``<Esc>ha
inoremap < <><Esc>ha
{% endhighlight %}



Each of the files that were called with the 'runtime' keyword is just another mini vim script that can be treated as a drop in replacement. I like storing my code in the modules foler.

[github-dotfiles-repo]: https://github.com/jude-shin/.dotfiles
[explaining-my-.dotfiles-repo]: httpps:google.com
