---
title: Cuda
date: 2017-08-4 12:47:24
author: Nick
categories: 高性能计算
tags: [GPU, Cuda, Ubuntu]
toc: true
---

## 下载
下载之前首先确定自己的电脑的显卡为Nvidia的显卡。其次是查询自己电脑的操作系统。
- 确定自己有NVIDIA的GPU。
```
$ lspci | grep -i nvidia
```
  如果有显示nvidia的显卡信息，则表示具备上述条件。一个符合条件的示例如下：
```
nick@seis:~$ lspci | grep -i nvidia
01:00.0 VGA compatible controller: NVIDIA Corporation Device 1b00 (rev a1)
01:00.1 Audio device: NVIDIA Corporation Device 10ef (rev a1)
```
- 确定自己的操作系统是cuda官网列出的支持的操作系统：
```
$ uname -m && cat /etc/*release
```
  官网上列出的有Fedora, OpenSUSE, RHEL, CentOS, SLES, Ubuntu。一个符合条件的示例如下：
```
nick@seis:~$ uname -m && cat /etc/*release
x86_64
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
DISTRIB_DESCRIPTION="Ubuntu 16.04.1 LTS"
NAME="Ubuntu"
VERSION="16.04.1 LTS (Xenial Xerus)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 16.04.1 LTS"
VERSION_ID="16.04"
HOME_URL="http://www.ubuntu.com/"
SUPPORT_URL="http://help.ubuntu.com/"
BUG_REPORT_URL="http://bugs.launchpad.net/ubuntu/"
UBUNTU_CODENAME=xenial

```
确认好这些信息后，从[官网](https://developer.nvidia.com/cuda-downloads)上下载对应的安装文件，这里选择下载是runfile(local)文件。

下载完成后，确认文件是否完整：
```
$ md5sum cuda_8.0.61_375.26_linux.run
33e1bd980e91af4e55f3ef835c103f9b  cuda_8.0.61_375.26_linux.run
```

## 预处理
删除之前安装的关于nvidia的软件
```
$ sudo apt-get purge nvidia-cuda*
```
如果想安装显卡驱动的话，则采用下面的命令:
```
$ sudo apt-get purge nvidia-*
```

## 安装Nvidia显卡驱动
1. 更新系统
```
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get dist-upgrade
```
2. 添加Nvidia显卡驱动源
```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
```
   运行该命令会警告是否添加，按回车继续即可。

3. 更新系统源
```
$ sudo apt-get update
```
4. 通过`Ctrl+Alt+F2`进入终端界面；
5. 停止使用图形界面:
```
$ sudo service lightdm stop
```
6. 在`/etc/modprobe.d/`目录下新建`blacklist-nouveau.conf`文件，并在该文件中写入下列信息:
```
blacklist nouveau
options nouveau modeset=0
```
7. 重新生成内核initrd:
```
$ sudo update-initramfs -u
```

8. 搜索最新的驱动并安装
```
$ apt-cache search nvidia*
$ sudo apt-get install nvidia-384 # 目前为最新版本
```
9. 重启
```
$ sudo reboot
```

## 安装Cuda8.0
运行安装文件`cuda_8.0.61_375.26_linux.run`:
```
$ sudo sh cuda_8.0.61_375.26_linux.run --override
```
对于安装过程中出现的选择, 除了安装显卡驱动那个选n，其他的全部选择y。

## 环境变量设置

在`~/.bashrc`文件最后添加下列语句
```
# CUDA-8.0
export PATH=$PATH:/usr/local/cuda/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-8.0/lib64
```
然后再终端执行下述语句完成环境变量的设置。
```
source ~/.bashrc
```

## 验证安装是否成功
- 显示nvidia版本

```
$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2016 NVIDIA Corporation
Built on Tue_Jan_10_13:22:03_CST_2017
Cuda compilation tools, release 8.0, V8.0.61
```

- 显示驱动版本以及GPU的内存信息
```
$ nvidia-smi
Fri Aug  4 15:12:38 2017       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 375.26                 Driver Version: 375.26                    |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  TITAN X (Pascal)    Off  | 0000:01:00.0      On |                  N/A |
| 23%   33C    P8    12W / 250W |    224MiB / 12188MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+

+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID  Type  Process name                               Usage      |
|=============================================================================|
|    0     14522    G   /usr/lib/xorg/Xorg                             179MiB |
|    0     15124    G   compiz                                          43MiB |
+-----------------------------------------------------------------------------+
```
- 运行示例文件
进入`NVIDIA_CUDA-8.0_Samples`文件夹下（默认在`$HOME`目录下），运行`make`进行编译，
编译需要花费一定的时间。编译完成后，进入`bin/x86_64/linux/release`目录，随便找一个
可执行文件，这里选择`deviceQuery`,运行该文件，若得到类似于下面的结果，则表示安装无误。
```
$ ./deviceQuery
./deviceQuery Starting...

 CUDA Device Query (Runtime API) version (CUDART static linking)

Detected 1 CUDA Capable device(s)

Device 0: "TITAN X (Pascal)"
  CUDA Driver Version / Runtime Version          8.0 / 8.0
  CUDA Capability Major/Minor version number:    6.1
  Total amount of global memory:                 12188 MBytes (12780175360 bytes)
  (28) Multiprocessors, (128) CUDA Cores/MP:     3584 CUDA Cores
  GPU Max Clock rate:                            1531 MHz (1.53 GHz)
  Memory Clock rate:                             5005 Mhz
  Memory Bus Width:                              384-bit
  L2 Cache Size:                                 3145728 bytes
  Maximum Texture Dimension Size (x,y,z)         1D=(131072), 2D=(131072, 65536), 3D=(16384, 16384, 16384)
  Maximum Layered 1D Texture Size, (num) layers  1D=(32768), 2048 layers
  Maximum Layered 2D Texture Size, (num) layers  2D=(32768, 32768), 2048 layers
  Total amount of constant memory:               65536 bytes
  Total amount of shared memory per block:       49152 bytes
  Total number of registers available per block: 65536
  Warp size:                                     32
  Maximum number of threads per multiprocessor:  2048
  Maximum number of threads per block:           1024
  Max dimension size of a thread block (x,y,z): (1024, 1024, 64)
  Max dimension size of a grid size    (x,y,z): (2147483647, 65535, 65535)
  Maximum memory pitch:                          2147483647 bytes
  Texture alignment:                             512 bytes
  Concurrent copy and kernel execution:          Yes with 2 copy engine(s)
  Run time limit on kernels:                     Yes
  Integrated GPU sharing Host Memory:            No
  Support host page-locked memory mapping:       Yes
  Alignment requirement for Surfaces:            Yes
  Device has ECC support:                        Disabled
  Device supports Unified Addressing (UVA):      Yes
  Device PCI Domain ID / Bus ID / location ID:   0 / 1 / 0
  Compute Mode:
     < Default (multiple host threads can use ::cudaSetDevice() with device simultaneously) >

deviceQuery, CUDA Driver = CUDART, CUDA Driver Version = 8.0, CUDA Runtime Version = 8.0, NumDevs = 1, Device0 = TITAN X (Pascal)
Result = PASS

```

## 可能出现的问题
1. 安装完成后，下次登录系统的时候出现循环登录而进不去图形界面。
```
这种情况一般情况下是显卡驱动不是最新版本造成的，建议安装最新版本的显卡驱动。
```
## 参考资料
https://askubuntu.com/questions/799184/how-can-i-install-cuda-on-ubuntu-16-04
http://docs.nvidia.com/cuda/cuda-installation-guide-linux/#axzz4HIBXnwyt
http://www.52nlp.cn/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0%E4%B8%BB%E6%9C%BA%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-ubuntu-16-04-nvidia-gtx-1080-cuda-8#comments
