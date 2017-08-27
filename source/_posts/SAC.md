---
title: SAC
date: 2015-05-16
author: SeisMan
categories: 地震学软件
tags: [SAC]
toc: true
---

## 前言

本文源于[SeisMan](http://seisman.info/)开源项目[SAC_Docs_zh](https://github.com/seisman/SAC_Docs_zh)

## 申请SAC

SAC协议仅允许IRIS将SAC源码包及二进制包分发给地震学相关人员。
所以想要从官方渠道获取SAC软件包，必须在 IRIS 网站上申请。

SAC软件包申请地址：<http://ds.iris.edu/ds/nodes/dmc/forms/sac/>

申请的过程中需要注意如下几点：

-   认真填写个人信息，否则可能会被拒绝
-   电子邮箱最好填写学术/单位邮箱（比如以 edu 结尾的邮箱），一般邮箱可能会被拒绝
-   若无学术邮箱，则需要其他信息证明你是地震学相关人员

IRIS提供了SAC最新版的源码包、Linux 64位下的二进制包和Mac OSX 64位下的
二进制包。其中，二进制包可以在相应的平台下直接使用，源代码包则需要编译
才能使用。具体申请那个包由用户的操作系统决定：

-   Linux 64位系统可以申请源码包或Linux 64位包
-   Mac OSX 64位系统可以申请源码包或Mac 64位包
-   其他系统，如Linux 32位、Mac OSX 32位、Cygwin，申请源码包

提交申请之后，需要人工审核，若审核通过，则IRIS会通过邮件将软件包
发送给你。一般审核时间为两到三个工作日。由于审核周期稍长，建议同时
申请64位二进制包和源码包。

需要注意，SAC协议规定了用户没有分发SAC软件包的权利。所以，请勿将SAC
软件包在网络上公开。

## 在Linux下安装SAC

Linux下安装SAC，可以直接使用官方提供的二进制包，也可以手动编译源码包。
对于大多数用户而言，建议安装二进制包。下面会分别介绍两种安装方法，要求
读者了解Linux的一些基本概念和操作。

### 安装二进制包

#### 安装依赖包

官方提供的二进制包中的可执行文件可以直接使用，在运行时需要用到几个动态
链接库。大部分Linux发行版下，都默认安装了这几个动态链接库。若不幸没有
安装或不确定有没有安装，可以通过如下命令安装所需的软件包。

对于Ubuntu/Debian：

```
$ sudo apt-get update
$ sudo apt-get install libc6 libsm6 libice6 libxpm4 libx11-6
$ sudo apt-get install zlib1g libncurses5
```

对于CentOS/Fedora/RHEL：

```
$ sudo yum install glibc libSM libICE libXpm libX11
$ sudo yum install zlib ncurses
```

#### 安装二进制包

直接将官方提供的二进制包解压并移动到安装目录即可：

```
$ tar -xvf sac-101.6a-linux_x86_64.tar.gz   # 解压
$ sudo mv sac /usr/local                    # 安装
```

### 编译源码

#### 安装依赖包

编译源码时需要安装若干软件包。

对于Ubuntu/Debian系：

```
$ sudo apt-get update
$ sudo apt-get install build-essential
$ sudo apt-get install libncurses5-dev libsm-dev libice-dev
$ sudo apt-get install libxpm-dev libx11-dev zlib1g-dev
```

对于CentOS/Fedora/RHEL系：

```
$ sudo yum install gcc gcc-c++ make
$ sudo yum install glibc ncurses-devel libSM-devel libICE-devel
$ sudo yum install libXpm-devel libX11-devel zlib-devel
```

#### 编译源码

将源码按如下命令解压、配置、编译、安装：

```
$ tar -xvf sac-101.6a_source.tar.gz
$ cd sac-101.6a
$ mkdir build
$ cd build
$ ../configure --prefix=/usr/local/sac
$ make
$ sudo make install
```

### 配置变量

向 `~/.bashrc` （某些发行版需要修改 `~/.bash_profile`） 中加入如下语句
以配置环境变量和SAC全局变量：

``` {.bash}
export SACHOME=/usr/local/sac
export SACAUX=${SACHOME}/aux
export PATH=${SACHOME}/bin:${PATH}

export SAC_DISPLAY_COPYRIGHT=1
export SAC_PPK_LARGE_CROSSHAIRS=1
export SAC_USE_DATABASE=0
```

其中，

-   `SACHOME` 为SAC的安装目录
-   `SACAUX` 目录中包含了SAC运行所需的辅助文件
-   `PATH` 为Linux系统环境变量
-   `SAC_DISPLAY_COPYRIGHT` 用于控制是否在启动SAC时显示版本和版权
    信息，一般设置为1。在脚本中多次调用SAC时会重复显示版本和版权信息，
    干扰脚本的正常输出，因而在脚本中一般将其值设置为0。具体的设置方法
    可以参考 [在脚本中调用SAC](/call-in-script) 中的相关内容
-   `SAC_PPK_LARGE_CROSSHAIRS` 用于控制震相拾取过程中光标的大小， 在
    [震相拾取](/data-process/picking-phase.md) 时会用到
-   `SAC_USE_DATABASE` 用于控制是否允许将SAC格式转换为GSE2.0格式，
    一般用不到该特性，故而设置其值为0

修改完 `~/.bashrc` 后，执行以下命令使配置的环境变量生效：

```
$ source ~/.bashrc
```

### 启动SAC

终端键入小写的 `sac`，显示 如下则表示SAC安装成功：

```
$ sac
 SEISMIC ANALYSIS CODE [11/11/2013 (Version 101.6a)]
 Copyright 1995 Regents of the University of California

SAC>
```

## 在Mac下安装SAC

本节介绍如何在Mac OS X 如何安装SAC。

Mac下安装SAC，可以直接使用官方提供的二进制包，也可以手动编译源码包
（似乎在最新版本的 macOS 下手动编译的 SAC 无法使用，暂无解决办法）。
对于大多数用户而言，建议安装二进制包。下面会分别介绍两种安装方法。

### 准备工作

首先要安装Mac下的命令行工具。在终端执行如下命令：

``` {.console}
$ xcode-select --install
```

即可。

此外，还需要安装X11图形界面相关工具，即 XQuartz。可以直接到
[XQuartz](http://xquartz.macosforge.org/landing/)
下载dmg安装包并安装。对于 Homebrew 用户，可以使用

``` {.console}
$ brew cask install xquartz
```

安装。

### 安装二进制包

直接将官方的二进制包解压并移动到安装目录即可：

``` {.console}
$ tar -xvf sac-101.6a-mac_x86_64.tar.gz
$ sudo mv sac /usr/local
```

### 编译源码

按照如下命令即可正确编译源码。需要注意的是，由于SAC默认使用的editline库
在Mac下无法正常编译，因而执行 `configure` 时使用了 `–enable-readline`
选项使得SAC使用readline库而不是editline库。

``` {.console}
$ tar -xvf sac-101.6a_source.tar.gz
$ cd sac-101.6a
$ mkdir build
$ cd build
$ ../configure --prefix=/usr/local/sac --enable-readline
$ make
$ sudo make install
```

### 配置变量

向 `~/.bash_profile` 中加入如下语句以配置环境变量和SAC全局变量：

``` {.bash}
export SACHOME=/usr/local/sac
export SACAUX=${SACHOME}/aux
export PATH=${SACHOME}/bin:${PATH}

export SAC_DISPLAY_COPYRIGHT=1
export SAC_PPK_LARGE_CROSSHAIRS=1
export SAC_USE_DATABASE=0
```

其中，

-   `SACHOME` 为SAC的安装目录
-   `SACAUX` 目录中包含了SAC运行所需的辅助文件
-   `PATH` 为Linux系统环境变量
-   `SAC_DISPLAY_COPYRIGHT` 用于控制是否在启动SAC时显示版本和版权
    信息，一般设置为1。在脚本中多次调用SAC时会重复显示版本和版权信息，
    干扰脚本的正常输出，因而在脚本中一般将其值设置为0。具体的设置方法
    可以参考 [在脚本中调用SAC](/call-in-script) 中的相关内容
-   `SAC_PPK_LARGE_CROSSHAIRS` 用于控制震相拾取过程中光标的大小， 在
    [震相拾取](/data-process/picking-phase.md) 时会用到
-   `SAC_USE_DATABASE` 用于控制是否允许将SAC格式转换为GSE2.0格式，
    一般用不到该特性，故而设置其值为0

修改完 `~/.bash_profile` 后，执行以下命令使配置的环境变量生效：

``` {.console}
$ source ~/.bash_profile
```

### 启动SAC

终端键入小写的 `sac`，显示如下则表示SAC安装成功：

``` {.console}
$ sac
 SEISMIC ANALYSIS CODE [11/11/2013 (Version 101.6a)]
 Copyright 1995 Regents of the University of California

SAC>
```
