---
title: Git
date: 2016-05-11

---

## SVN与Git的最主要的区别

SVN是集中式版本控制系统，版本库是集中放在中央服务器的，而干活的时候，用的都是自己的电脑，所以首先要从中央服务器哪里得到最新的版本，然后干活，干完后，需要把自己做完的活推送到中央服务器。集中式版本控制系统是必须联网才能工作，如果在局域网还可以，带宽够大，速度够快，如果在互联网下，如果网速慢的话，就纳闷了。

Git是分布式版本控制系统，那么它就没有中央服务器的，每个人的电脑就是一个完整的版本库，这样，工作的时候就不需要联网了，因为版本都是在自己的电脑上。既然每个人的电脑都有一个完整的版本库，那多个人如何协作呢？比如说自己在电脑上改了文件A，其他人也在电脑上改了文件A，这时，你们两之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

### 创建证书使用公钥免密码登录(可选)
	ssh-keygen -t rsa
	vi ~/.ssh/authorized_keys

## 配置git提交用户名和邮箱，定义别名方便区分
	git config --global user.name "whuper"
	git config --global user.email "whuper@163.com"

## 初始化Git仓库
git init --bare sample.git

## 在客户端上克隆远程仓库
git clone git@server:/srv/sample.git

## 测试推送
	touch README
	git add README
	git commit -m "add readme"
	git push origin master



## 常用命令
	git status
	git diff readme.txt
	git log 

查看远程分支.

	git branch -a
	
查看本地分支

	git branch
	git checkout -b dev origin/dev，作用是checkout远程的dev分支，在本地起名为dev分支，并切换到本地的dev分支
切换到本地分支

	git checkout develop

删除本地分支

	git branch -D release

丢弃本地修改

	git reset  --hard

删除远程分支

	git branch -r -d origin/branch-name

	git push origin :branch-name
撤销某一个提交

	git revert <SHA> 

### 合并提交记录

合并最近两条: git rebase -i head～2

### git 如何删除缓存的远程分支列表

	git remote prune origin
## Git忽略规则及.gitignore规则不生效的解决办法

	git rm -r --cached .
	git add .
	git commit -m 'update .gitignore'


### or create a new repository on the command line
	echo "# compilations" >> README.md
	git init
	git add README.md
	git commit -m "first commit"
	git remote add origin https://github.com/whuper/compilations.git
	git push -u origin master


### or push an existing repository from the command line
	git remote add origin https://github.com/whuper/compilations.git
	git push -u origin master

## git 免密码