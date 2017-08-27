---
title: SPECFEM2D
date: 2016-05-14 18:13:54
tags: [SEM, SPECFEM2D]
categories: 地震学软件
toc: true
---

## 下载

要下载SPECFEM2D，可以在终端中输入以下命令：

``` {.console}
$ git clone --recursive --branch devel https://github.com/geodynamics/specfem2d.git
```

## 安装

安装SPECFEM 2D需要安装

+ [ifort编译器](intel.html)
+ [mpi并行环境](mpich.html)
+ [scotch](scotch.html)


有了上述工具之后，我们可以进入specfem2d的根目录进行安装。

### Configuration

``` {.console}
$ ./configure FC=ifort MPIFC=mpif90
```

### Compilation

``` {.console}
$ make
```

编译完成后，可以在specfem2d的根目录下发现有一个新的目录bin/,该目录下包含specfem2d的二进制可执行文件。
