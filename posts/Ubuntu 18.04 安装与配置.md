<!--
.. title: Ubuntu 18.04 安装与配置
.. slug: ubuntu18.04-安装与配置
.. date: 2019-04-19 20:41:53 UTC+08:00
.. tags: Linux, Ubuntu 
.. category: Linux
.. link: 
.. description: 
.. type: text 
.. has_math: false
-->

[TOC]

# 1 安装Ubuntu

## 1.2 引导盘

- 下载Ununtu 18.04的镜像文件，将U盘格式化为FAT32，然后解压操作系统的镜像文件至U盘根目录，完成刻录。
- 下载Ubuntu 18.04的镜像文件，使用Rufus进行刻录。（使用默认设置即可）

## 1.3 关闭Windows的快速启动

进入控制面板 -> 电源选项 -> 选择关闭电源按钮功能 -> 更改当前不可用设置：关闭启用快速启动，保存设置。

## 1.4安装系统

重启计算机，在开机画面出现之前，狂按f12进入bios启动界面，选择从U盘启动，选择install Ubuntu。

一些注意事项：

- 安装时选择最小安装
- 安装过程中选择使用语言时选择英语，便于在命令行中使用cd命令
- 选择区域和城市：Asia/Shanghai

# 2 必要配置

## 2.1 更换软件源

Ununtu 18.04默认使用的是国外的软件源，因为使得我们在下载和更新软件时速度较慢，这里我们替换为国内的阿里云软件源：

点击Software&Updates -> Ununtu Software -> 点击download from右边的下拉框，选择Other -> 在弹出的界面中选择阿里云软件源。

## 2.2 全面更新

在进一步操作之前，我们先把已经安装的软件包都升级到最新版，在终端输入：

```shell
sudo apt update
sudp apt upgrade
```

点击settings -> language -> Manage Installed Language，完成语言列表的更新，然后注销重启。

## 2.3 显卡配置

如果电脑带Nvidia独立显卡，点击Software&Updates -> Additional Drivers里选择对应的N卡私有的驱动程序并应用更改。

## 2.4 更换终端类型

更换默认终端为fish[https://launchpad.net/~fish-shell/+archive/ubuntu/release-3]

在终端输入以下命令：

```shell
sudo apt-add-repository ppa:fish-shell/release-3
sudo apt-get update
sudo apt-get install fish
```

