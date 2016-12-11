---
title: MacVim 安装 vim-airline 插件
date: 2016-12-11 18:24:55
tags: [Vim]
---

> Emacs 是神的编辑器，而 Vim 是编辑器之神。

这句话由来已久，具体的出处已经不可考了。

要真讲起来，我使用编辑器之神也有小几年了，但是一直以来都只是把它当成普通的编辑器来使用了。这无异于使用屠龙刀来杀鸡，每次想起来都让我感学羞愧难当，这次痛定思痛，决定再把 Vim 系统学习一遍，务必把它当成日常使用的编辑器。

Vim 编辑器最重要的两样零件就是 .vimrc 配置文件和各种好用的插件了。

关于 .vimrc 配置文件，我决定从零开始，在重新学习的过程中一行一行添加自己需要的功能，而非直接使用网上各种大神共享的配置文件，毕竟使用别人的东西难免还是会不趁手。

而对于 Vim 插件，现在已经有很多好用的插件管理器了，安装插件也不是什么困难的事了。

但是，在我安装第一个插件 [vim-airline](https://github.com/vim-airline/vim-airline.git) 的时候就遇到了不少坑，这里用篇文章来记录下。

<!-- more -->

## Vim 插件管理器

就像上面讲过的，现在网上已经有很多好用的插件管理器了，比如 [Vundle](https://github.com/VundleVim/Vundle.vim.git)、[NeoBundle](https://github.com/Shougo/neobundle.vim.git) 和 [vim-plug](https://github.com/junegunn/vim-plug.git)。

对于新手来讲，随便选一个来使用就行了。我自己使用的是更轻量，速度更快的 vim-plug。

## 安装 vim-plug

vim-plug 的安装非常简单，只需要一行命令：

```bash
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

## 使用 vim-plug 安装 vim-airline

vim-plug 插件管理器安装完成之后，就可以开始安装插件了。

我选择安装的第一个插件是 [vim-airline](https://github.com/vim-airline/vim-airline.git)，它是一个状态栏插件，可以显示当前文件的很多信息，更重要的是它的效果够酷炫，安装完之后可以让整个 Vim 的逼格上升一个层次。

使用 vim-plug 安装插件十分简单，只需要在 .vimrc 文件中添加如下内容：

```bash
" vim-plug
call plug#begin('~/.vim/plugged')

Plug 'vim-airline/vim-airline'

call plug#end()
```

然后执行 `:PlugInstall` 命令，坐等插件安装完成就可以了。

### 配置始终显示状态栏

安装完 vim-airline 之后，迫不及待发打开编辑器，结果发现状态栏并没有显示。赶紧放狗搜索，原来默认情况下 Vim 的状态栏只有在有两个窗口的情况下才会显示，如果需要让状态栏始终显示，需要在 .vimrc 文件中打开如下选项：

```bash
:set laststatus=2
```

### 安装补丁字体

现在状态栏可以显示了，但是你会发现它的效果很差劲。这是因为用系统自带的字体会有很多图形符号无法显示，所以为了获得更酷炫的效果，我们需要安装补丁字体。

幸好补丁字体的安装过程也不复杂，首先[到这里](https://github.com/powerline/fonts.git)下载补丁字体，下载完之后执行目录中的 `install.sh` 脚本：

```bash
$ cd fonts
$ ./install.sh
```

这样补丁字体就安装好了。


### 配置 Vim 字体

字体安装完成之后，接下来就是配置 Vim 使用的字体了，注意这里我们只配置图形化界面的 Vim，即 MacVim，命令行下的 Vim 会默认使用控制台的字体。

首先，在 Vim 下执行命令：

```bash
:set guifont=*
```

这时会弹出一个字体选择器，选择你喜欢的字体，注意要选择后缀带有 **For Powerline** 字体，只有这些字体才是补丁字体，才能正确地显示图形化符号。

选择完成点击确定后，再在 Vim 下执行如下命令：

```bash
:set guifont?
```

这时在编辑器最底下会显示当前使用的字体，把这行字记下来，然后添加到 .vimrc 文件中。同时还要设置 Vim 的编码为 utf-8，如下所示：

```bash
set encoding=utf-8

if has("gui_running")
    set guifont=Meslo\ LG\ S\ Regular\ for\ Powerline:h14
endif
```

### 配置 vim-airline

设置完补丁字体之后，最后一步就是再配置一下 vim-airline 了。

在 .vimrc 中添加如下内容：

```bash
let g:airline_powerline_fonts = 1

if !exists('g:airline_symbols')
  let g:airline_symbols = {}
endif

let g:airline_left_sep = ''
let g:airline_left_alt_sep = ''
let g:airline_right_sep = ''
let g:airline_right_alt_sep = ''
let g:airline_symbols.branch = ''
let g:airline_symbols.readonly = ''
let g:airline_symbols.linenr = '¶'
let g:airline_symbols.crypt = '🔒'
let g:airline_symbols.paste = '∥'
```

大功告成！现在快打开你的 Vim 看下效果吧。

关于 air-line 还有很多其它的选项，不过默认的选项就已经符合我目前的使用需求了。更加详细的配置，可以参考它的[官方文档](https://github.com/vim-airline/vim-airline)

下面是我最终配置完成的效果图：

![My vim-airline](http://7xqonv.com1.z0.glb.clouddn.com/macvim-install-airline-plugin-pic-1.png)

## 小结

用好 Vim 最重要的一点就是练习，而且是刻意练习，将 Vim 的常用操作都变成肌肉记忆，才能在一瞬间完成在其它人看起来像是巫术的操作。

这里是[我的 .vimrc 配置文件](https://github.com/buginux/vim-configuration)，现在里面还没有多少内容，不过我会在学习过程中逐渐增加内容的。
