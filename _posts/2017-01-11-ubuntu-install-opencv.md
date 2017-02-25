---
layout: post
title: Ubuntu14.04下安装OpenCV-2.4.13
categories: [Ubuntu, Computer Vision]
tags: OpenCV
---

1、先安装一些依赖项：

    sudo apt-get install build-essential
    sudo apt-get install libgtk2.0-dev libavcodec-dev libavformat-dev  libtiff4-dev  libswscale-dev libjasper-dev

2、安装cmake，可以使用apt-get安装：

    sudo apt-get install cmake

apt-get安装的cmake版本可能有点老，建议从[官网](https://cmake.org/download/)下载最新版本源码编译安装

3、安装pkg-config，它是一个提供从源代码中编译软件时查询已安装的库时使用的统一接口的计算机软件:

    sudo apt-get install pkg-config

4、从[OpenCV官网](http://opencv.org/downloads.html)下载opencv-2.4.13

5、编译安装：

    cd opencv-2.4.13
    mkdir release
    cd release
    cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..  
    make -j7
    sudo make install
