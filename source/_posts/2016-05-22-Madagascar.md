---
title: Madagascar
date: 2016-05-22 18:12:01
modified: 2017-07-18 14:08:01
tags: [RSF, Madagascar]
categories: 地震学软件
toc: true
---

与Seismic Unix类似，Madagascar也是一款地震数据处理软件，其开发者众多，内置的软件也非常多，
缺点就是代码注释比较少，不容易懂。给的示例虽多，但也不容易懂，对于初学者而言十分困难。

下载最新的Release版本：<http://github.com/ahay/src.git>

## 安装依赖包

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

## 编译，安装

``` {.console}
$ git clone https://github.com/ahay/src.git madagascar-2.0
$ cd madagascar-2.0
$ sudo ./configure API=c++,f90 --prefix=/opt/rsf2.0 # 额外安装C++，Fortran两个API
$ sudo make
$ sudo make install
```

## 配置环境变量

向`~/.bashrc`中加入如下语句以配置环境变量。

``` {.bash}
# Madagascar 2.0
export RSFROOT=/opt/rsf2.0
source $RSFROOT/share/madagascar/etc/env.sh
```

- 2016-05-22: 初稿；
- 2017-07-18: 更新至madagascar2.0
