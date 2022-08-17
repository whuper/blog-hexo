---
title: Hello Hexo

date : 2017-05-08

---
# Hello Hexo

Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

```
$ hexo new "My New Post"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

```
$ hexo server
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

```
$ hexo generate
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

```
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)

------

### 安装hexo

npm install -g hexo

初始化Hexo

```
hexo init
```

在_config.yml,进行基础配置(博客名，作者，主题)

### 常用命令

```
hexo g

hexo s
hexo clean && hexo s
```

在_config.yml,进行基础配置（deployment）

安装hexo-deployer-git自动部署发布工具

```
npm install hexo-deployer-git  --save
```

发布到Github

```
hexo clean && hexo g && hexo d
```

https://hexo.io/docs/deployment.html)
