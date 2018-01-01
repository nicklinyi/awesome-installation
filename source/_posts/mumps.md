---
title: MUMPS
date: 2016-05-22 18:08:49
tags: MUMPS
categories: 数学库
toc: true
---

## 下载

下载,需要上下面的网站去预留信息，然后开发者会在几小时内将软件发至你留的邮箱中，建议下载最新版本5.1.1。

源码：<http://graal.ens-lyon.fr/MUMPS/> , 如果该链接无法访问，请访问 <https://launchpad.net/ubuntu/+source/mumps/5.1.1-2> , 该网页中有个名为`mumps_5.1.1.orig.tar.gz`,下载该文件即可。

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
$ tar zxvf MUMPS_5.1.1.tar.gz
$ cd MUMPS_5.1.1
```

### 修改Makefile.inc

安装好intel编译器后，在此可直接复制 `Make.inc/` 目录下的 `Makefile.INTEL.PAR` 文件至 MUMPS 的根目录，并修改一些参数使之与自己的系统相符。

``` console
$ cp Make.inc/Makefile.INTEL.PAR ./Makefile.inc
```

由于 MUMPS 需要依赖与 METIS 或 SCOTCH 来划分网格，两种选一种即可，此处选择的是 METIS，下载 METIS 并编译好 METIS 后，需要修改刚才的`Makefile.inc`文件，具体改动如下：

去掉`LMETISDIR`和`LMETIS`前的注释，并根据自己METIS的安装情况给其赋值，然后，在添加`-Dmetis`在`ORDERINGSF`之后。 例如：

```makefile
#
#  This file is part of MUMPS 5.1.1, released
#  on Mon Mar 20 14:34:33 UTC 2017
#
#Begin orderings

# NOTE that PORD is distributed within MUMPS by default. It is recommended to
# install other orderings. For that, you need to obtain the corresponding package
# and modify the variables below accordingly.
# For example, to have Metis available within MUMPS:
#          1/ download Metis and compile it
#          2/ uncomment (suppress # in first column) lines
#             starting with LMETISDIR,  LMETIS
#          3/ add -Dmetis in line ORDERINGSF
#             ORDERINGSF  = -Dpord -Dmetis
#          4/ Compile and install MUMPS
#             make clean; make   (to clean up previous installation)
#
#          Metis/ParMetis and SCOTCH/PT-SCOTCH (ver 6.0 and later) orderings are recommended.
#

#SCOTCHDIR  = ${HOME}/scotch_6.0
#ISCOTCH    = -I$(SCOTCHDIR)/include
#
# You have to choose one among the following two lines depending on
# the type of analysis you want to perform. If you want to perform only
# sequential analysis choose the first (remember to add -Dscotch in the ORDERINGSF
# variable below); for both parallel and sequential analysis choose the second
# line (remember to add -Dptscotch in the ORDERINGSF variable below)

#LSCOTCH    = -L$(SCOTCHDIR)/lib -lesmumps -lscotch -lscotcherr
#LSCOTCH    = -L$(SCOTCHDIR)/lib -lptesmumps -lptscotch -lptscotcherr


LPORDDIR = $(topdir)/PORD/lib/
IPORD    = -I$(topdir)/PORD/include/
LPORD    = -L$(LPORDDIR) -lpord

################## 修改部位1 ###################
LMETISDIR = $(HOME)/software/metis/lib
IMETIS    = $(HOME)/software/metis/include
###############################################

# You have to choose one among the following two lines depending on
# the type of analysis you want to perform. If you want to perform only
# sequential analysis choose the first (remember to add -Dmetis in the ORDERINGSF
# variable below); for both parallel and sequential analysis choose the second
# line (remember to add -Dparmetis in the ORDERINGSF variable below)

################## 修改部位2 ###################
LMETIS    = -L$(LMETISDIR) -lmetis
#LMETIS    = -L$(LMETISDIR) -lparmetis -lmetis
###############################################

# The following variables will be used in the compilation process.
# Please note that -Dptscotch and -Dparmetis imply -Dscotch and -Dmetis respectively.
# If you want to use Metis 4.X or an older version, you should use -Dmetis4 instead of -Dmetis
# or in addition with -Dparmetis (if you are using parmetis 3.X or older).

################## 修改部位3 ###################
#ORDERINGSF = -Dscotch -Dmetis -Dpord -Dptscotch -Dparmetis
ORDERINGSF  = -Dpord -Dmetis
ORDERINGSC  = $(ORDERINGSF)
###############################################

LORDERINGS = $(LMETIS) $(LPORD) $(LSCOTCH)
IORDERINGSF = $(ISCOTCH)
IORDERINGSC = $(IMETIS) $(IPORD) $(ISCOTCH)

#End orderings
########################################################################
################################################################################

PLAT    =
LIBEXT  = .a
OUTC    = -o
OUTF    = -o
RM = /bin/rm -f
CC = mpiicc
FC = mpiifort
FL = mpiifort
AR = ar vr
#RANLIB = ranlib
RANLIB  = echo
# Make this variable point to the path where the Intel MKL library is
# installed. It is set to the default install directory for Intel MKL.
MKLROOT=/opt/intel/mkl/lib/intel64
LAPACK = -L$(MKLROOT) -lmkl_intel_lp64 -lmkl_intel_thread -lmkl_core
SCALAP = -L$(MKLROOT) -lmkl_scalapack_lp64 -lmkl_blacs_intelmpi_lp64

LIBPAR = $(SCALAP) $(LAPACK)

INCSEQ = -I$(topdir)/libseq
LIBSEQ  = $(LAPACK) -L$(topdir)/libseq -lmpiseq

LIBBLAS = -L$(MKLROOT) -lmkl_intel_lp64 -lmkl_intel_thread -lmkl_core
LIBOTHERS = -lpthread

#Preprocessor defs for calling Fortran from C (-DAdd_ or -DAdd__ or -DUPPER)
CDEFS   = -DAdd_

#Begin Optimized options
OPTF    = -O -nofor_main -qopenmp # or -openmp for old compilers
OPTL    = -O -nofor_main -qopenmp
OPTC    = -O -qopenmp
#End Optimized options

################## 修改部位4 ###################
INCS = $(INCPAR) -I$(IMETIS)
###############################################

LIBS = $(LIBPAR)
LIBSEQNEEDED =

```

### Compilation

``` {.console}
$ make alllib
```

编译完成后，可以在MUMPS的安装目录下发现有目录lib/,该目录主要用于链接外部函数调用此库文件。
{% alert warning %}
运行 `make alllib` 之后，可能会报错，错误信息为'ar: two different operation options specified'，其主要原因在于 Makefile.inc 文件中 AR 变量的设置存在问题。需要在后面补充一个空格符，即可完成正常安装。
{% endalert %}
