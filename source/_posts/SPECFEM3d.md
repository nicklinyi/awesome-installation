---
title: SPECFEM3D
date: 2016-05-15 18:13:54
tags: [SEM, SPECFEM3D]
categories: 地震学软件
toc: true
---

## 下载

要下载SPECFEM3D，可以在终端中输入以下命令：

``` {.console}
$ git clone --recursive --branch devel https://github.com/geodynamics/specfem3d.git
```
## 可选依赖包
以下安装包均为可选。
+ [ifort编译器](intel.html)，可使用 gfortran 代替。
+ [mpi并行环境](mpich.html)，可安装不带 MPI 加速的版本
+ [scotch](scotch.html)，安装SPECFEM3D时会自动安装其目录下绑定的scotch版本，但版本不是最新版本


上述工具中建议安装 ifort 编译器，使用 gfortran 编译，可能会出现一些问题且编译效率低。

## 安装
进入specfem3d的根目录进行安装。
### 安装不带MPI的版本

``` {.console}
$ ./configure FC=ifort --without-mpi
$ make
```

### 安装带MPI的版本
``` {.console}
$ ./configure FC=ifort MPIFC=/opt/mpich3.2/bin/mpif90 MPI_INC=/opt/mpich3.2/include --with-mpi
$ make
```
{% alert warning %}
注意：这里最好显式地给出`mpif90`的路径，不然编译的时候会调用系统里安装的其他`mpif90`,然后就是给出MPI的include变量`MPI_INC`，编译的时候会用到`mpi.h`头文件，所以需要从`MPI_INC`里找。
{% endalert %}

### 安装带 MPI 且使用非 SPECFEM3D 绑定的 scotch 版本
和安装带 MPI 的版本的差不多，只不过，采用的是下面的configure
```
$ ./configure FC=ifort MPIFC=/opt/mpich3.2/bin/mpif90 MPI_INC=/opt/mpich3.2/include --with-mpi -with-scotch-dir=/opt/scotch_6.0.4
$ make
```
{% alert warning %}
注意：本文给出的`/opt/mpich3.2`,`/opt/scotch_6.0.4`只是个例子，大家根据自己 mpich 和 scotch 的安装位置进行适当修改，没有必要一定要将 mpich 和 scotch 安装在`/opt`下。
{% endalert %}

编译完成后，可以在 specfem3d 的根目录下发现有一个新的目录 `bin/`,该目录下包含 specfem3d 的二进制可执行文件。
