---
title: SEPlib
date: 2016-05-19 16:45:30
updated: 2016-05-23 12:38:00
author: Nick
categories: 地震学软件
tags: [SEPlib, Linux, 地震学软件]
toc: true
---

## 软件下载

下载最新版本的 SEPlib，目前为6.5.3. 实际有7.0.5，但官网上没有链接了，估计人都转向 Madagascar 了。

下载地址： <http://sepwww.stanford.edu/doku.php?id=sep:software:installing_seplib>


## 安装依赖包

安装 SEPlib-6.5.3，需要一些依赖包，由于笔者之前安装过 Madagascar 以及SU，因此 大部分的依赖包基本安装完毕，但还需要安装下列依赖包:

    $ sudo apt-get install flex  # lex
    $ sudo apt-get install libpawlib-lesstif3-dev # lesstif

这里需要注意的是`lesstif`包。不同版本的Debian系统可能含有的软件包不一样，具体以自己版本为主。

## 基础安装
最好使用`ifort`来编译代码，在`configure`前需要显式指出下列环境变量:

    $ export CC=icc      # C 编译器
    $ export FC=ifort    # Fortran 编译器
    $ export CFCDEFINES=-DpgiFortran # 如果不设置此项，后面链接finitpar会出错

另外，一点需要注意的是，如果使用`ifort`来编译，运行下面的命令后，需要查看`ifort`是否已经设置为Fortran编译器，这点很重要，因为一般如果将 SEPlib 安装在`/opt`下的话，需要使用`root`权限，而我们一般将`ifort`的环境变量设置在自己的用户目录下，因此`configure`时检查`ifort`会出现没有`ifort`而采用系统的一些Fortran编译器。



    $ ./configure --prefix=/opt/SEPlib --with-su=/su/installed/dir --with-local

`configure`完成后，在终端键入下面命令，将编译 SEPlib，并将其安装至指定地文件目录下

    $ make
    $ make install

在编译的过程中可能出现哑元类型不匹配的问题，主要原因是因为源程序`super_chain_mod.f90`部分代码输入参数定义不严谨所致，修改该程序中部分代码即可解决此类问题。需要修改的部分如下所示.

    integer function flat_chain_exec( adj, add, model, data) ! W Grad K
      integer       ::stat
      logical,intent(in)      :: adj, add                                ! 28
      real          :: data(:), model(:)
    integer function flat_chain_adj_exec( adj, add, model, data) ! K' Grad' W'
      integer       ::stat
      ! logical       :: adj, add,adj_new
      logical,intent(in)        :: adj, add                              ! 51
      logical       :: adj_new                                           ! 52
      real          :: data(:), model(:)
    integer function flat3_chain_exec( adj, add, model, data)
      integer       ::stat
      logical,intent(in)        :: adj, add                              ! 115
      real          :: data(:), model(:)
    integer function flat3_chain_adj_exec( adj, add, model, data)

      integer       ::stat
      logical,intent(in)        :: adj, add                              ! 142
      logical       :: adj_new                                           ! 143

## 环境变量的设置

环境变量的设置，这一步是非常重要的，正确设置环境变量对以后正常使用大有裨益。在此，笔者借鉴了 Madagascar 的环境变量设置方案，可以在 SEPlib 的安装目录`SEPROOT`下的`share`目录中建立一文件夹名叫`seplib`（读者可自行取其他名字），根据用户自己的`shell`类型来创建不同的环境变量执行脚本，对于`csh`用户，创建`env.csh`；而对`bash`用户，则创建`env.sh`.下面分别给出不同的`shell`类型的脚本，供大家参考，至于具体目录文件的设置，根据自己的喜好设置。

`csh`文件

    #!/bin/csh
    # Path for SEPlib installation directory
    setenv SEPROOT /opt/seplib

    # Path for SEPlib source directory
    setenv SEPSRC /home/abc/seplib-6.5.3

    # Path for Python packages
    if ($?PYTHONPATH) then
    setenv PYTHONPATH $SEPROOT/lib/python:${PYTHONPATH}
    else
    setenv PYTHONPATH $SEPROOT/lib/python
    endif

    # Path for binary data files part of SEP datasets
    # setenv DATAPATH /var/tmp/

    # Path for manual pages
    setenv MANPATH `manpath`:$SEPROOT/man

    # Path for shared object files
    # if ($?LD_LIBRARY_PATH) then
    # setenv LD_LIBRARY_PATH $SEPROOT/lib:${LD_LIBRARY_PATH}
    # else
    # setenv LD_LIBRARY_PATH $SEPROOT/lib
    # endif

    # Path for executables
    set path = ($SEPROOT/bin $path)

    # Path for vplot files
    setenv VPLOTSPOOLDIR /var/tmp/SEP/Figs/

    # Path for vplot fonts
    setenv VPLOTFONTDIR $SEPROOT/include

    # Path for SEP includedir
    setenv SEPINC $SEPROOT/include

`bash`文件

    #!/bin/sh

    # Path for SEPlib installation directory
    export SEPROOT=/opt/seplib

    # Path for SEPlib source directory
    export SEPSRC=/home/abc/seplib-6.5.3

    # Path for Python packages
    if [ -n "$PYTHONPATH" ]; then
    export PYTHONPATH=$SEPROOT/lib/python:${PYTHONPATH}
    else
    export PYTHONPATH=$SEPROOT/lib/python
    fi

    # Path for binary data files part of SEP datasets
    # export DATAPATH=/var/tmp/

    # Path for manual pages
    unset MANPATH
    export MANPATH=`manpath`:$SEPROOT/man

    # Path for shared object files
    # if [ -n "$LD_LIBRARY_PATH" ]; then
    # export LD_LIBRARY_PATH=$SEPROOT/lib:${LD_LIBRARY_PATH}
    # else
    # export LD_LIBRARY_PATH=$SEPROOT/lib
    # fi

    # Path for executables
    export PATH=$SEPROOT/bin:$PATH

    # Path for vplot files
    export VPLOTSPOOLDIR=/var/tmp/SEP/Figs/

    # Path for vplot fonts
    export VPLOTFONTDIR=$SEPROOT/include

    # Path for SEP includedir
    export SEPINC=$SEPROOT/include

这样设置好了之后，我们需要修改自己的`~/.bashrc`文件,在文件最后面添加如下几句,这样环境变量就设置好了。

    # SEPlib 6.5.3
    export SEPROOT=/opt/seplib
    source $SEPROOT/share/seplib/env.sh

修改`~/.bashrc`文件之后，我们只需要在当前终端键入如下命令，即可使环境变量马上生效:

    $ source ~/.bashrc

## 安装更多功能的SEPlib

在成功安装了不带 FFTW 以及 MPI 库的 SEPlib. 下面介绍安装一个更完整的 SEPlib。

由于对 SEPlib 的源代码以及整个程序框架有了更进一步的了解，所以这次选择更高级一点的安装方式。首先在安装源文件的根目录执行如下命令:

	$ cp docs/config-examples/RUN_THIS.base .

然后修改该文件，整体文件不再大改，只需要根据自己的需求修改其中的某些变量即可。由于这次主要是加入 OPENMP 和 FFTW。之所以没安装 MPI 主要是因为 OPENMP 适合于共享式内存计算机。下面列出一些基本的参数设置信息

	setenv CC gcc
	setenv FC ifort
	setenv FFTW_FCLD "-L/usr/lib/x86_64-linux-gnu/ -lfftw3 -lfftw3f" # 根据自己 FFTW 的位置修改
	setenv OMP_FCFLAGS -openmp # -openmp 为 Intel 编译器的选项，其他编译器不一样，需修改成该编译器的 OPENMP 选项
	setenv OMP_FCLD "-L/opt/intel/compilers_and_libraries_2016/linux/lib/intel64/ -liomp5 -liompstubs5" # 根据自己　Intel openmp 动态链接库的位置修改
	./configure --prefix=/opt/SEPlib --with-su=/su/installed/dir --with-local --with-fftw --with-omp

修改完成后，保存文件，在终端对文件添加可执行权限,并执行脚本:

	$ chmod +x RUN_THIS.base
	$ ./RUN_THIS.base ＃运行脚本

键入上述命令后,将会编译整个源文件，在编译的过程中遇到一些问题，主要原因是源文件的 Makefile 文件存在一些问题。这里列出本人安装过程中出现的一些错误：

Bug1: 编译`Transf_fftw`时出错，出现`undefined reference omp_get_num_threads`

解决: 修改`Transf_fftw.f90`所在文件目录的 Makefile 文件，在484行末尾，即`Transf_fftw_LDADD`这行末尾添加`${OMP_FCLD}`，然后在504,506,508,511行中加入`${OMP_FCFLAGS}`,修改完保存，再回到根目录重新`$make`一下就好了。上述 Bug1 在后面还有几次编译过程仍存在此问题，解决办法是找到出错的源文件位置，按照上面提到的解决方案，添加 openmp 动态链接库位置以及编译选项，即可解决此类问题。

Bug2: 编译`RickMovie`时会出现错误，主要是类似于`undefined reference Xtadd/X11...`这样的错误。

解决: 进入`RickMovie`目录，修改`Makefile`文件，修改其中的第212行，即`Rickmovie_LDADD`所在那行，在其后添加`-lXt -lX11`即可解决此类问题，其实思路与前面omp一致，无非是没有将相关库加入编译选项中。Bug2 在后面的编译中还会遇到，主要位于`Ricksep`中，运用同样方法可以解决，不再赘述。

解决完所有的Bug1、Bug2之类的错误后，后面的编译过程基本畅通无阻，`$ make`完之后再`$ make install`，即可安装完　SEPlib，最后再设置环境变量即可正常使用。
