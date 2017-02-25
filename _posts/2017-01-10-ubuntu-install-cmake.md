---
layout: post
title: Ubuntu14.04下安装cmake-3.7.1
categories: [Ubuntu]
tags: Ubuntu
---

直接使用apt-get安装的cmake版本为2.8，有点老，可参考本方法安装最新版本

1、[官网](https://cmake.org/download/)下载最新版本源码（source），目前为3.7.1

2、安装

    tar xf cmake-3.7.1.tar.gz

    cd cmake-3.7.1

    ./configure

    make

    sudo make install
