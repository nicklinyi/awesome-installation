---
title: MPICH
date: 2016-05-26 10:07:42
author: Nick
categories: 高性能计算
tags: MPI
toc: true
---

首先要知道的是 MPI 并不是一种编程语言，只是一种并行库，用来指示编译器自己的程序中哪些地方需要并行，哪些地方正常串行等等。目前常用的有 MPICH 和 OPENMPI 两个 MPI 库，这里选择 MPICH 来进行安装。

其次， MPICH 最好从源码编译，这样一来可以得到和自己机器相匹配的最优性能，二来会避免今后可能出现的一些错误（我测试过直接安装 Debian 自带的 MPICH 包，结果安装过程没问题，编译程序，运行程序会出现一些错误，可能与机器相关吧）

## 下载
MPICH最新版本目前为 3.2， 下载地址为：<https://www.mpich.org/downloads/>

## 安装

进入到 MPICH 的下载目录，然后解压文件
```
tar zxvf mpich-3.2.tar.gz
```
然后切换至 MPICH 的源文件目录，运行`configure`,并指定安装目录。
```
cd mpich-3.2
./configure --prefix=/opt/mpich3.2
```
`configure`完成之后，直接`$ make`可编译 MPICH 源文件目录下的所有文件，接着`# make install`就可以将 MPICH 安装在指定的安装目录。

注：这里我没有指定 C 和 Fortran 的编译器，因为 MPICH3.2 默认采用 Intel 的编译器来进行编译源文件，如果没有 Intel 编译器也会自动`configure`系统自带的一些编译器来进行编译，如果想手动指定编译器的话可以设置`FC`,`CC`等环境变量，然后再进行编译
