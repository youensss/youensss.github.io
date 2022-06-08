<!--
.. title: Ubuntu 美化
.. slug: ubuntu-美化
.. date: 2019-04-19 20:41:53 UTC+08:00
.. tags: Linux, Ubuntu 
.. category: Linux
.. link: 
.. description: 
.. type: text 
.. has_math: false
-->

[TOC]

# 安装Gnome-tweak-tool

Ubuntu 18.04 默认使用的桌面为gnome3。打开终端执行：

```
sudo apt-get install gnome-tweak-tool
```

# User theme插件安装

使shell主题可以使用桌面主题

# 改变桌面主题与图标主题

- [桌面主题](https://github.com/vinceliuice/vimix-gtk-themes)
- [图标主题](https://github.com/vinceliuice/vimix-icon-theme)
- [壁纸](https://wallhaven.cc/)

进入Github下载源文件，解压到目录并进入，右键点击空白处选择在终端中打开，执行安装。待安装完成后在Gnome-tweak-tool里选择桌面主题与图标主题。

# 安装搜狗输入法

搜狗输入法依托fcitx输入法框架，因此我们首先安装fcitx框架。打开终端执行：

```
sudo apt install fcitx -y
```

然后进入搜狗输入法官网下载搜狗输入法的deb安装包。安装搜狗输入法。

打开gnome-tweaks，在开机启动推荐中添加fcitx。



