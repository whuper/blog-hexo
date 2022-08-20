---

title: mac-vim
date: 2017-03-30

---

## macvim配置

安装macvim

### 查看预装vim版本

```
vim --version
```

### 查看预装vim路径

```
where vim
```

### 安装

有两种方式来安装macvim:

- Github上下载macvim.dmg安装包进行安装
- 使用Homebrew安装

brew的方式：

```
brew install macvim
```

### 建立软链接

#### 为macvim中的vim创建别名，将其添加至~/.bash_profile配置文件

```
alias mvim='/Applications/MacVim.app/Contents/MacOS/MacVim'
```

或将可执行文件mvim复制到/usr/local/bin/路径下

```
cp /Applications/MacVim.app/Contents/MacOS/MacVim /usr/local/bin/mvim
```

或者在/usr/local/bin/路径中为mvim建立软链接

```
ln -s /Applications/MacVim.app/Contents/MacOS/MacVim /usr/local/bin/mvim
```

### source ~/.bash_profile

完成！～

### 安装Vundle，在终端输入以下代码即可

```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

### .vimrc

```
mkdir ~/.vim    
touch ~/.vimrc  
```

### linux/mac版的vim中如何用快捷键进行与系统剪切板交互的复制粘贴

```
"+y
"+p
```

------

上述命令不能用的话是因为vim的clipboard选项没有打开

```
$ vim --version |grep clipboard #查看该选项 +clipboard表示选项开启，-clipboard表示未开启
```

可以下载源码重新编译，在configure的时候增加 –with-features=huge

------

### 附 linux源码方式编译安装vim

配置选项

```
./configure --with-features=huge --enable-pythoninterp=yes  --enable-cscope \
 --enable-luainterp --with-lua-prefix=/usr/local/  --enable-fontset --enable-perlinterp --enable-rubyinterp \
--with-python-config-dir=/usr/lib/python2.6/config \
--disable-gui --prefix=/opt/local/vim  
```


----


## Vim 正则表达式替换替换 



删除文本中所有中文字符

:%s/\v[^\x00-\xff]+//g


删除行首数字

:%s/^[0-9]\{1,\}//g

删除/后面的字符

:%s/^\/.*$//g

:%s/\/.*$//g

删除包含数字的行

:g/(\d|^P)/d
g/^P/d 
