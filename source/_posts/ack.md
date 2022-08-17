---
title: vim安装ack
date: 2018-01-12

---

# vim安装ack(brew安装)

### brew

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)" 
```

------

```
brew update  更新brew；

brew install {应用名，如git} 安装软件
```

Ack.vim主要用来在项目里全局搜索某个单词,比如搜索函数名的时候.

实现类似Ctrl + shift + f 全局搜索的功能

安装ack:

```
brew install ack
```

安装ack.vim插件:

进入./vim/bundle目录:
bundle安装

在.vimrc中加入

```
Plugin ‘mileszs/ack.vim’
```

执行

```
:BundleInstall
```

此时已可以使用ack命令;

:Ack xxx * ,在项目里搜索xxx单词.
每次手动输入:Ack xxx还是很不方便的

可以在.vimrc文件里设置快捷键：

```
map <Leader>f :Ack! -i<Space>
```

-i 参数表示忽略大小写. 以后在项目里,只需要按`,+f`,即可全局搜索单词了.
