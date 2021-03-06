---
title: SCOTCH
date: 2017-05-22 18:05:56
tags: SCOTCH
categories: 数学库
toc: true
---

## 下载
Scotch最新版本目前为 6.0.4， 下载地址为：<https://gforge.inria.fr/projects/scotch>

## 安装依赖包

安装scotch依赖于make工具以及`lex`和`yacc`，可以通过如下命令确定自己的系统是否包含上述工具。

``` {.console}
$ make --version
$ which lex
$ which yacc
```

如果没有，可以通过apt包管理系统进行安装

``` {.console}
$ sudo apt install make
$ sudo apt install flex   # install lex
$ sudo apt install bison  # install yacc
```

## Configuration

1.在src/目录下创建`Makefile.inc`文件。首先进入`src/`下，然后再进入`Make.inc/`子目录寻找适合自己系统的文件，如果您的系统是`x86_64`构架的，而且也安装了icc编译器，那么您可以选择`Makefile.inc.x86-64_pc_linux2.icc`文件。并回到src/目录下，建立软链接如下：

``` {.console}
$ ln -s Make.inc/Makefile.inc.x86-64_pc_linux2.icc Makefile.inc
```

## 安装 scotch

一旦您建立了自己的Makefile.inc文件，那么安装scotch只是一个很简单的命令

``` {.console}
$ make scotch          #将scotch安装在src/上一级目录下
```

## 安装 ptscotch
安装ptscotch首先需要MPI环境，因此需要安装MPICH或OPENMPI。安装MPICH的步骤[点击这里](mpich-install.html)，安装OPENMPI的步骤可参考[SeisMan](http://seisman.info/linux-environment-for-seismology-research.html)。
有了MPI环境后, 需要对刚才新建的`Makefile.inc`进行修改，由于文件比较小，这里贴上我修改过的`Makefile.inc`文件。
```Makefile
EXE		=
LIB		= .a
OBJ		= .o

MAKE		= make
AR		= ar
ARFLAGS		= -ruv
CAT		= cat
CCS		= icc
CCP		= mpicc
CCD		= icc
INC     = /opt/mpich3.2/include  # 新增此行， 根据自己MPICH的安装目录修改
CFLAGS		= -O3 -I$(INC) -DCOMMON_FILE_COMPRESS_GZ -DCOMMON_PTHREAD -DCOMMON_RANDOM_FIXED_SEED -DSCOTCH_RENAME -DSCOTCH_PTHREAD  -DIDXSIZE64 # 去掉了 -restrict 选项，不然编译会报错
CLIBFLAGS	=
LDFLAGS		= -lz -lm -lrt -pthread -lirc # 新增 INTEL 的 irc 库文件，不然编译会报错
CP		= cp
LEX		= flex -Pscotchyy -olex.yy.c
LN		= ln
MKDIR		= mkdir
MV		= mv
RANLIB		= ranlib
YACC		= bison -pscotchyy -y -b y
```
修改上述地方后，`$ make ptscotch` 进行ptscotch的安装。

## 检查是否安装成功
``` {.console}
$ make check
```
