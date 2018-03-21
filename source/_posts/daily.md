---
title: 常用软件
date: 2017-08-11
author: Nick
categories: 常用软件
tags: [安装, 软件]
toc: true
---

<!--more-->

##  Chrome

    $ wget https://dl.google.com/linux/direct/google-chrome-stable_current_i386.deb # 下载 32bit
    $ wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb # 下载 64bit
    $ sudo dpkg -i google-chrome-stable_current_amd64.deb

安装过程中如遇到错误提示,直接执行以下命令来安装 Chrome 浏览器需要的类库

    $ sudo apt-get -f install

## Mendeley
Mendeley 是一个跨平台的文献管理软件,其内部自带了一个可以添加注释的 PDF 阅读器。下载链接:<https://www.mendeley.com/download-mendeley-desktop>
    安装:

        $ sudo apt-get install libqtwebkit-dev # 安装依赖包
        $ sudo dpkg -i mendeleydesktop_1.15.1-stable_amd64.deb
        $ cd /opt/mendeleydesktop/bin
        $ ./install-mendeley-link-handler.sh /opt/mendeleydesktop/bin/mendeleydesktop

## 搜狗输入法

Linux 下有两大输入法框架:ibus 和 fcitx,其中 fcitx 的体验要比 ibus 好,因此选择
fcitx 框架,并安装搜狗输入法。

    $ sudo apt-get install fcitx

与此同时,我们可以上搜狗官网下载搜狗输入法 linux 版:<http://pinyin.sogou.com/linux/>
下载完搜狗输入法的安装包后,我们切换至下载此安装包的目录,进行安装

    $ sudo dpkg -i sogoupinyin_2.1.0.0068_amd64.deb

安装完后,需要 logout 再 login，然后在终端中输入

    $ fcitx-config-gtk3

在 `Input Method` 栏中,点击 `+`, 然后选择搜狗输入法(Sogou Pinyin)。设置好后,采
用快捷键 `Ctrl+Space` 即可调出搜狗输入法。

## JDK1.8

1. 上Oracle官网下载JDK1.8
2. 把JDK安装到`/usr/lib/jvm`这个路径下。如果系统没有该路径则创建该路径。
把下载好的压缩包文件解压到`/usr/lib/jvm`这个路径下
```
sudo tar zxvf jdk-8u111-linux-x64.tar.gz -C /usr/lib/jvm
```
3. 配置环境变量，在用户根目录下的`.bashrc`文件中追加如下内容
```
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_111
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
```
4. 重启终端让其生效或者在当前终端下输入 `$source ~/.bashrc`

Ref:
http://www.linuxidc.com/Linux/2013-11/93012.htm
