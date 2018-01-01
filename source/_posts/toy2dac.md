---
title: TOY2DAC
date: 2016-05-22 18:02:47
categories: 地震学软件
tags: [toy2dac, FWI]
toc: true
---

## 下载

下载链接:<https://seiscope2.obs.ujf-grenoble.fr/TOY2DAC,82>

## 编译

将下载的软件解压后，进入`doc/`目录下，查看manual文档进行编译。

## 依赖包安装

安装TOY2DAC需要MUMPS来进行大型稀疏矩阵的LU分解，而MUMPS又要安装METIS或者SCOTCH，同时，编译MUMPS来依赖 SCALAPACK, BLACKS, LAPACK 三个依赖库文件，好在这些都在Intel的MKL库中集成，因此实际上只需要安装Intel编译器，MPI，METIS，MUMPS。这几个软件的安装可参考如下链接：

- [Intel编译器](intel.html)
- [MPI库](mpich.html)
- [METIS](metis.html)
- [MUMPS](mumps.html)


## 修改Makefile.inc

安装完上述依赖包后，进入`src/`，找到`Makefile.inc`文件并编辑该文件，需要修改上述库文件的`LIB`和`INC`为其安装目录。原文件需要改动的地方不多。笔者主要修改了如下几处：

``` {.makefile}

#ROOT DIR OF MUMPS/METIS
LADIR = ~/software

#ROOT DIR OF TOOL BOX
LTOOLS_BOX = ~/software/TOOLBOX_OPTIMIZATION

#MUMPS LIB AND INC
LMUMPS = -L$(LADIR)/MUMPS_5.1.1/lib -lcmumps -lmumps_common
IMUMPS = -I$(LADIR)/MUMPS_5.1.1/include

#PORD LIB (INSIDE MUMPS)
LPORDDIR = $(LADIR)/MUMPS_5.1.1/PORD/lib/
LPORD    = -L$(LPORDDIR) -lpord

#METIS LIB
LMETISDIR = $(LADIR)/metis/lib
LMETIS    = -L$(LMETISDIR) -lmetis

#TOOL BOX LIB AND INC
LOPTIM = -L $(LTOOLS_BOX)/lib -lSEISCOPE_OPTIM
IOPTIM  = -I $(LTOOLS_BOX)/COMMON/include

#WE GATHER EVERYTHINGS
LIBPAR = $(LMUMPS) $(LPORD) $(LOPTIM)  $(LMETIS)  $(LMKL)
INCPAR = $(IMUMPS) $(IPORD) $(IOPTIM) -I../include

INC = $(INCPAR)
LIB = $(LIBPAR)

```

### Compilation

在`src/`目录下，运行 `make`，便完成整个编译过程
