---
title: MUMPS
date: 2016-05-22 18:08:49
tags: MUMPS
categories: 数学库
toc: true
---

## 下载

下载,需要上下面的网站去预留信息，然后开发者会在几小时内将软件发至你留的
邮箱中，建议下载最新版本5.0.1。

源码：<http://graal.ens-lyon.fr/MUMPS/>

## 基本依赖包

安装MUMPS有三种方式，一种是只安装单核版(sequential version);一种是安装多核版(multicore machine)；最后一种是并行版(parallel version).由于笔者安装MUMPS的主要目的是用于FWI，因此对于运行速度要求很高，在此选择并行版进行安装。其它方式的安装比较简单，读者可自行尝试。

安装并行版MUMPS需要以下一些依赖包

-   MPI

-   BLAS library

-   BLACS library

-   ScaLAPACK library

其中 MPI 一般有两种 MPICH 和 OPENMPI, 安装 MPICH [请点击这里](mpich.html)；至于BLAS，BLACS，ScaLAPACK 这三个库可使用 Intel 的 MKL 库，因此需要安装 intel 的编译器，[请点击这里](intel.html)。

### 解压

``` {.console}
$ tar zxvf MUMPS_5.0.1.tar.gz
$ cd MUMPS_5.0.1
```

### 修改Makefile.inc

安装好intel编译器后，在此可直接复制 `Make.inc/` 目录下的 `Makefile.INTEL.PAR` 文件至 MUMPS 的根目录，并修改一些参数使之与自己的系统相符。

``` {.console}
$ cp Make.inc/Makefile.INTEL.PAR ./Makefile.inc
```

由于MUMPS需要依赖与METIS或SCOTCH来划分网格，两种选一种即可，此处选择的是METIS，下载METIS并编译好METIS后，需要修改刚才的Makefile.inc文件，具体改动如下：

去掉LMETISDIR和LMETIS前的注释，并根据自己METIS的安装情况给其赋值，然后，在添加-Dmetis在ORDERINGSF之后。 例如：

``` {.makefile}
LMETISDIR = ~/software/metis/lib
LMETIS    = -L$(LMETISDIR) -lmetis
ORDERINGSF  = -Dpord -Dmetis
```

### Compilation

``` {.console}
$ make
```

编译完成后，可以在MUMPS的安装目录下发现有目录lib/,该目录主要用于链接外部函数调用此库文件。
