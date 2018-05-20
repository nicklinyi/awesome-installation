---
title: Madagascar
date: 2015-05-14 18:12:01
modified: 2018-05-20 14:08:01
tags: [RSF, Madagascar]
categories: 地震学软件
toc: true
---



与Seismic Unix类似，Madagascar也是一款地震数据处理软件，其开发者众多，内置的软件也非常多，
缺点就是代码注释比较少，不容易懂。给的示例虽多，但也不容易懂，对于初学者而言十分困难。

下载最新的Release版本：<http://github.com/ahay/src.git>


## Linux 下安装Madagascar

### 安装依赖包

``` {.console}
# 必须安装的依赖包
$ sudo apt-get install libxaw7-dev
# 建议安装的依赖包
$ sudo apt-get install freeglut3-dev
$ sudo apt-get install libnetpbm10-dev
$ sudo apt-get install libgd-dev  
$ sudo apt-get install libplplot-dev
$ sudo apt-get install libavcodec-dev
$ sudo apt-get install libcairo2-dev
$ sudo apt-get install libjpeg-dev
$ sudo apt-get install swig
$ sudo apt-get install python-dev
$ sudo apt-get install python-numpy
$ sudo apt-get install g++
$ sudo apt-get install gfortran
$ sudo apt-get install libopenmpi-dev
$ sudo apt-get install libfftw3-dev
$ sudo apt-get install libsuitesparse-dev
# 不建议安装的包
$ sudo apt-get install python-epydoc
```

### 编译，安装

``` {.console}
$ git clone https://github.com/ahay/src.git madagascar-2.0
$ cd madagascar-2.0
$ ./configure API=c++,f90,python,matlab --prefix=/home/lloyd/rsf2.0 # 额外安装C++，Fortran, Python, Matlab四个API
$ make
$ make install
```

### 配置环境变量

向`~/.bashrc`中加入如下语句以配置环境变量。

``` {.bash}
# Madagascar 2.0
export RSFROOT=/opt/rsf2.0
source $RSFROOT/share/madagascar/etc/env.sh
```

## macOS High Sierra 下安装Madagascar
### 安装依赖包
- 安装[MacPorts](https://www.macports.org/),进官网，下载安装文件打开安装即可。
- 安装Xcode，从App Store里安装即可
- X11，可通过安装[Xquartz](https://www.xquartz.org/)来获得X11，进官网下载安装文件安装即可
```
## FFTW
$ sudo port install fftw-3-single
$ sudo port install fftw-3 +gfortran
## MPI
sudo port install openmpi
sudo port select --set mpi openmpi-mp-fortran
## ppmpen
sudo port install libnetpbm
```

### 编译，安装

``` {.console}
## 在Madagascar 源码的根目录下执行下列操作
$ ./configure CC=clang CXX=clang++ API=python,f90 --prefix=/Users/lloyd/rsf # 额外安装python，Fortran两个API
$ make
$ make install
```

### 配置环境变量

向`~/.bash_profile`中加入如下语句以配置环境变量。

``` {.bash}
# Madagascar 2.0
export RSFROOT=/Users/lloyd/rsf
source $RSFROOT/share/madagascar/etc/env.sh
```

- 2016-05-22: 初稿；
- 2017-07-18: 更新至madagascar2.0
- 2018-04-28: 更新macOS High Sierra下安装Madagascar
- 2018-05-20: 更新安装Matlab API


