---
title: 终端常用的快捷键
date: 2017-10-11

---

# terminal

### 终端常用的快捷键：

```
Ctrl + d        删除一个字符，相当于通常的Delete键（命令行若无所有字符，则相当于exit；处理多行标准输入时也表示eof）
Ctrl + h        退格删除一个字符，相当于通常的Backspace键
Ctrl + u        删除光标之前到行首的字符
Ctrl + k        删除光标之前到行尾的字符
Ctrl + c        取消当前行输入的命令，相当于Ctrl + Break
Ctrl + a        光标移动到行首（Ahead of line），相当于通常的Home键
Ctrl + e        光标移动到行尾（End of line）
Ctrl + f        光标向前(Forward)移动一个字符位置
Ctrl + b        光标往回(Backward)移动一个字符位置
Ctrl + l        清屏，相当于执行clear命令
Ctrl + p        调出命令历史中的前一条（Previous）命令，相当于通常的上箭头
Ctrl + n        调出命令历史中的下一条（Next）命令，相当于通常的上箭头
Ctrl + r        显示：号提示，根据用户输入查找相关历史命令（reverse-i-search）

次常用快捷键：
Alt + f         光标向前（Forward）移动到下一个单词
Alt + b         光标往回（Backward）移动到前一个单词
Ctrl + w        删除从光标位置前到当前所处单词（Word）的开头
Alt + d         删除从光标位置到当前所处单词的末尾
Ctrl + y        粘贴最后一次被删除的单词
```

------

### 查找

```
find . -name "*蝙蝠侠*"

# 找出当前目录以及其所有子目录下所有名字中包含“蝙蝠侠”三字的文件

find . -name "*.rmvb" -maxdepth 1
# 找出当前目录（不包括子目录）下所有名字中后缀为".rmvb"的文件
```

mac下，有个locate命令，它自动建立和维护文件的索引，所以找起来非常快

例如：要找redis

```
locate redis
```

mac 显示隐藏文件
打开终端，输入：

```
defaults write com.apple.finder AppleShowAllFiles -bool true       此命令显示隐藏文件
defaults write com.apple.finder AppleShowAllFiles -bool false      此命令关闭显示隐藏文件
```

命令运行之后需要重新加载Finder：快捷键option+command+esc，选中Finder，重新启动即可

Mac 更新系统后无法使用git

解决方案：

在终端输入：

```
xcode-select --install
```

据说原因是因为每次更新系统之后xcode就被卸载了，因此需要重新安装一次。特此记录，以便查阅

如果需要禁止spotlight, 在终端中运行命令：

```
sudo mdutil  -a -i off 
```

开启

```
sudo mdutil -a -i on 
```

有的时候，它可能会造成问题，需要先关闭再开启，让他重新建立索引，可以运行命令：

```
sudo mdutil -E 
```

为了查看它的状态，可以运行：

```
sudo mdutil -s 
```
