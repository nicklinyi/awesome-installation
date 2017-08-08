---
title: TOY2DAC
date: 2016-05-22 18:02:47
categories: 地震学软件
tags: [toy2dac, FWI]
toc: true
---

### 下载

下载链接:<https://seiscope2.obs.ujf-grenoble.fr/TOY2DAC,82>

### 编译

将下载的软件解压后，进入`doc/`目录下，查看manual文档进行编译。

### 依赖包安装

安装TOY2DAC需要MUMPS来进行大型稀疏矩阵的LU分解，而MUMPS又要安装METIS或者SCOTCH，同时，编译MUMPS来依赖 SCALAPACK, BLACKS, LAPACK 三个依赖库文件，好在这些都在Intel的MKL库中集成，因此实际上只需要安装Intel编译器，MPI，METIS，MUMPS。这几个软件的安装可参考如下链接：

- Intel编译器: <http://seisman.info/intel-non-commercial-software.html>
- MPI库: <http://seisman.info/linux-environment-for-seismology-research.html>
- METIS: <https://nicklinyi.github.io/blog/2016/05/metis.html>
- MUMPS: <https://nicklinyi.github.io/blog/2016/05/mumps.html>


### 修改Makefile.inc

安装完上述依赖包后，进入`src/`，找到`Makefile.inc`文件并编辑该文件，需要修改上述库文件的`LIB`和`INC`为其安装目录。原文件需要改动的地方不多。笔者主要修改了如下几处：

``` {.makefile}
CC = mpiicc
FC = mpiifort
FL = mpiifort

OPTC = -03

LADIR = ~/software/MUMPS_5.0.1
LMUMPS = -L  $(LADIR)/lib -lcmumps -lmumps_common

IPORD = -I$(LADIR)/PORD/include/
LMETIS=  -L ~/software/metis/lib -lmetis
LTOOLBOX= ~/software/TOOLBOX_OPTIMIZATION

INCPAR =  -I../include  $(IPORD) -I $(LADIR)/include/ $(IOPT)
```

#### Compilation

在`src/`目录下，运行 `make`，便完成整个编译过程
