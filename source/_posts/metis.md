---
title: METIS
date: 2016-05-22 18:07:26
tags: METIS
categories: 数学库
toc: true
---

## 下载

选择最新版本(5.1.0)进行下载：

源码链接：<http://glaros.dtc.umn.edu/gkhome/metis/metis/download>

## 基本依赖包

-   C compiler，比如 gcc

-   GNU make ,一般Linux系统自带

-   Cmake 2.8以上

### 修改头文件(可选)

修改`include/metis.h`文件中的`IDXTYPEWIDTH`，来指定系统浮点位宽，32bit 系统只能选择32, 64bit 系统即可选择32，也可选择64.具体根据自己需求决定

### Compilation

``` {.console}
$ make config shared=1 cc=gcc prefix=/where/to/install/metis
$ make install
```

编译完成后，可以在METIS的安装目录下发现有目录`lib/`,该目录主要用于链接外部函数调用此库文件。
