---
title: Debian升级系统至Stretch
date: 2017-08-04 12:47:24
author: Nick
categories: 系统
tags: [Linux, Debian]
toc: true
---

1. **做好数据备份(最重要)**
2. **升级Debian 8系统上的所有软件**
在升级到Debian 9系统之前，要把当前系统上的所有软件升级到最新版本，输入下面的命令：
```
$ sudo apt-get update
$ sudo apt-get upgrade -y
```
如果升级的过程中遇到个人软件下载存在问题，那就是源的问题，更换其他源即可解决。
3. **更新source.list软件源文件**
现在编辑Debian的apt软件源文件/etc/apt/sources.list，将文件中所有的jessie替换成stretch。我们可以用sed命令来实现。
```
$ sudo sed -i 's/jessie/stretch/g' /etc/apt/sources.list
```
4. **再次升级软件包**
在更新完软件源文件后，先升级软件包。这一步被称为最小化升级(minimal upgrade)。
```
$ sudo apt-get update
$ sudo apt-get upgrade -y
```
5. **升级系统版本至Debian 9 Stretch**
再升级系统版本，输入下面的命令。这一步被称为全体升级(full upgrade)。
```
$ sudo apt-get dist-upgrade -y
```
6.  **清除旧的依赖关系和软件安装包**
分别执行下面的两个命令
```
$ sudo apt-get autoremove
$ sudo apt-get clean
```
7. **验证系统版本**
现在你的系统应该成功地升级到了Debian 9。重启系统，然后检查系统版本。
```
# reboot
```
查看当前系统的版本
```
root@debian:~# lsb_release -a
No LSB modules are available.
Distributor ID:  Debian
Description:      Debian GNU/Linux 9.0 (stretch)
Release:            9.0
Codename:       stretch
```
