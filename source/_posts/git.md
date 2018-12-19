---
title: 同一台电脑git多帐号配置

date: 2018-12-19

---


> 在同一台电脑上使用github和gitlab，主要的思想就是使用不同的仓库时，切换成不同的账号。不同账号的sshKey分别对应github和gitlab。接下来跟着我看看怎么做吧_

## 一、生成ssh密钥

这里我们要做的事情就是分别对githubn和gitlab生成对应的密钥（默认情况下本地生成的秘钥位于/Users/用户名/.ssh/），并且配置git访问不同host时访问不同的密钥，流程如下：
 **1、** 在gitbash中使用`ssh-keygen -t rsa -C "wangwenh@**.com"`生成对应的gitlab密钥：*id_rsa*和*id_rsa.pub*
 **2、** 将gitlab公钥即*id_rsa.pub*中的内容配置到公司的gitlab上
 **3、** 在gitbash中使用`ssh-keygen -t rsa -C "whuper@163.com" -f ~/.ssh/github_rsa`生成对应的github密钥：*github_rsa*和*github_rsa.pub*
 **4、** 将github公钥即*github_rsa.pub*中的内容配置到自己的github上
 **5、** 进入密钥生成的位置，创建一个`config`文件，添加配置：

```sh
# gitlab
Host gitlab
    HostName git.xxx.com #这里填你的gitlab的Host
    User git
    IdentityFile ~/.ssh/id_rsa
# githab
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_rsa
```

## 二、测试连接

在密钥的生成位置/Users/用户名/.ssh/下使用gitbash运行`ssh -T git@hostName`命令测试sshkey对gitlab与github的连接：

```sh
catalinaLi@catalinaLi MINGW64 ~/.ssh
$ ssh -T git@gitlab
Welcome to GitLab, catalinaLi!

catalinaLi@catalinaLi MINGW64 ~/.ssh
$ ssh -T git@github.com
Hi catalinaLi! You've successfully authenticated, but GitHub does not provide shell access.
```

如果出现上图结果就说明连接成功，如果不是这样的话就仔细看看第一步哪里做错了。

