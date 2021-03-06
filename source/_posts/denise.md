---
title: DENISE
date: 2015-05-13 18:15:31
tags: DENISE
categories: 地震学软件
toc: true
---

DENISE是一个KIT开发的用于全波形反演的软件。

## 下载

下载,需要上下面的网站去预留信息，然后开发者会在几小时内将软件发至你留的邮箱中。

官网：<http://www.gpi.kit.edu/Software-FWI.php>

安装DENISE的过程十分简单，开发者把脚本都写好了，只需要运行一下代码就可以编译成功了。

## 编译manual手册

将下载的软件解压后，进入doc/目录下，运行`compile_LaTeX_manual.sh`编译manual文档，里边有一部分关于安装的说明。

``` {.console}
$ ./compile_LaTeX_manual.sh
```

## 安装依赖包

安装依赖包文件需要进入par/目录下运行make，即可成功安装四个依赖包cseife，stfinv，aff，fourier.并编译整个软件包，这一点与根目录下的INSTALL文件不同，原INSTALL文件中说的compileLIBRARIES.sh在par/目录下根本不存在，因此整个编译过程应参考manual文档

### Compilation

由于源码含F77代码，需要指定F77的编译器，在contrib/下的`Makefile_var`文件中指定。可根据自己系统的f77编译器确定。笔者使用的gfortran.

``` {.makefile}
# edit /DENISE-Release/contrib/Makefile_var
FC=/usr/local/gfortran
```

修改完成之后，回到par/目下执行make即可编译整个程序

``` {.console}
$ make
```
