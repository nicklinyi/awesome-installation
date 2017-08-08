---
title: Seismic Unix
date: 2016-05-22 18:11:00
tags: SU
categories: 地震学软件
toc: true
---

Seismic Unix是Colorado School of Mines开发的一款地震数据处理软件。最新的版本代号为44R1.

下载地址：<http://www.cwp.mines.edu/cwpcodes/index.html>

## 安装依赖包

``` {.console}
$ sudo apt-get install build-essential
$ sudo apt-get install libx11-dev
$ sudo apt-get install libxt-dev
$ sudo apt-get install freeglut3-dev  
$ sudo apt-get install libxmu-dev
$ sudo apt-get install libxi-dev
$ sudo apt-get install gfortran
```

## 配置环境变量

一般软件的安装都是先编译，之后配置环境变量，su不同，正好相反，需要先配置环境变量。

向`~/.bashrc`中加入如下语句以配置环境变量。

``` {.bash}
# Seismic Unix 44R1
export CWPROOT=/home/usrname/su
export PATH=$PATH:/home/usrname/su/bin
```

注：usrname对读者自己的用户名。不要以root权限安装su，若造成系统不稳定，后果自负!!!



## 编译，安装
编译前，可以选择修改一下`src/Makefile.config`文件。
```{.makefile}
LINEHDRFLAG =
#XDRFLAG =  -DSUXDR  建议将此行注释掉，此行的目的在于将所有的su文件都变成big-endian的格式
ENDIANFLAG = -DCWP_LITTLE_ENDIAN
LARGE_FILE_FLAG = -D_FILE_OFFSET_BITS=64 -D_LARGEFILE64_SOURCE
```
``` {.console}
$ make install
$ make xtinstall
$ make finstall
$ make mglinstall
```
