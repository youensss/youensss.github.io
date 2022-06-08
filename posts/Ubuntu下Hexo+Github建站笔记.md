<!--
.. title: Ubuntu 下 Hexo + Github 建站笔记
.. slug: ubuntu下Hexo+github建站笔记
.. date: 2019-07-04 16:19:48
.. tags: Ubuntu, Hexo, Github
.. category: Hexo
.. link:
.. description:
.. type: text
-->

[TOC]

本篇记录建站所用资料与建站过程中所遇到的问题。

# 1 初始化博客与本地测试

## 1.1 安装git

> [Git](https://git-scm.com/) is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

```
sudo apt-get install git
git --version # 查看git版本
```

## 1.2 安装Node.js

> 简单的说[Node.js](https://nodejs.org/en/)就是运行在服务端的 JavaScript。Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。Node.js是一个事件驱动I/O服务端JavaScript环境，基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。npm是Node.js的包管理工具，其方便高效，在Node.js安装时自动安装完成。npm可以根据依赖关系，把所有依赖的包都下载下来并管理起来。

使用apt-get命令安装：

```
sudo apt-get install nodejs
sudo apt-get install npm
```

```
node -v # 查看Node.js版本
npm -v # 查看npm版本
```

## 1.3 安装Hexo并初始化博客

> [Hexo](https://hexo.io/) is a fast, simple and powerful blog framework. You write posts in [Markdown](http://daringfireball.net/projects/markdown/) (or other languages) and Hexo generates static files with a beautiful theme in seconds.

新建博客目录Blog，进入到该目录下右键打开终端，输入以下命令：

```
npm install -g hexo-cli  # 将hexo安装到全局
hexo init # 在当前目录初始化博客
npm install hexo-deployer-git --save  # 安装部署插件
```

## 1.4 本地测试

终端输入：`hexo s`，按照终端提示在浏览器输入`http://0.0.0.0:4000`，即可访问首页。

# 2 链接到Github

## 2.1 配置git

1. 配置用户信息

```
git config --global user.name "你的GitHub用户名"
git config --global user.email "你的GitHub注册邮箱"
```

2. 配置ssh

```
ssh-keygen -t rsa -C "你的GitHub注册邮箱"
```

在终端输入上述指令后，直接三个回车，默认不需要设置密码，最后得到了两个文件：`id_rsa`和`id_rsa.pub`。（操作过程中国按终端提示选择`overwrite`即可）

在终端利用`cat /home/username/.ssh/id_rsa.pub`指令查看ssh密钥文件并复制，打开`Github_Settings_keys`页面，新建new SSH Key，将ssh密钥文件粘贴进去并添加。

在终端中检测GitHub公钥设置是否成功，输入 `ssh git@github.com`

> 这里之所以设置GitHub密钥原因是，通过非对称加密的公钥与私钥来完成加密，公钥放置在GitHub上，私钥放置在自己的电脑里。GitHub要求每次推送代码都是合法用户，所以每次推送都需要输入账号密码验证推送用户是否是合法用户，为了省去每次输入密码的步骤，采用了ssh，当你推送的时候，git就会匹配你的私钥跟GitHub上面的公钥是否是配对的，若是匹配就认为你是合法用户，则允许推送。这样可以保证每次的推送都是正确合法的。

## 2.2 在Github上建立仓库并配置

1. 创建仓库名为`用户名.github.io`的仓库。如果仓库名正确，拉到下方的GitHub Pages可以看到“Your site is published at:`http://用户名.github.io/`”。
2. 编辑站点配置文件

在博客目录下的`_config.yml`文件中找到以下代码并修改：

```
deploy:
  type: git
  repo:https://github.com/mader96/mader96.github.io.git
  branch: master
```

## 2.3 生成静态文件并部署

```
hexo g
hexo d
```

### 修改主题

我安装的是cactus主题，在themes目录下使用git命令将cactus主题克隆至本地，并在站点配置文件中修改themes为cactus。

更多主题[https://hexo.io/themes/]

# 3 hexo常用命令

```
npm install hexo -g   #安装hexo
npm update hexo -g    #升级hexo
hexo init             #初始化博客
hexo n  "我的博客"     #新建文章
hexo g                #生成静态文件
hexo s                #启动服务预览
hexo d                #部署
hexo clean            #清除缓存
hexo g -d
```

**参考资料**

- [hexo文档](<https://hexo.io/docs/>)
- [cactus主题](https://github.com/probberechts/hexo-theme-cactus)
- [知乎教程](<https://zhuanlan.zhihu.com/p/26625249>)
