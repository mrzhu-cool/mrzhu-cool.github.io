---
layout: post
title: Ubuntu14.04下安装Matlab-2015b
categories: [Ubuntu, Software]
tags: Ubuntu
---

1、挂在ISO镜像文件

    cd ～
    sudo mkdir /media/matlab
    sudo mount -o loop matlab_2015b/R2015b_glnxa64.iso /media/matlab

2、执行安装过程，选择不联网

    cd /media/matlab
    sudo ./install

序列号为：54422-40402-23817-20808-30933

3、破解

    cd ~
    sudo cp matlab_2015b/crack/libcufft.so.7.0.28 /usr/local/MATLAB/R2015b/bin/glnxa64 
    sudo cp matlab_2015b/crack/libmwservices.so /usr/local/MATLAB/R2015b/bin/glnxa64

4、激活

    cd /usr/local/MATLAB/R2015b/bin
    sudo ./matlab

采用不联网激活，找到crack中的激活文件Matlab_R2015b_glnxa64.lic,导入激活

5、快捷启动

    sudo apt-get install matlab-support

添加matlab的安装目录为/usr/local/MATLAB/R2015b/

按照提示安装完成之后，可以直接在命令行执行matlab启动，也可以执行桌面快捷方式启动

如果出现runtime error，是因为安装的时候使用了sudo（root权限），而直接输入matlab启动时，是没有root权限的。解决方法为：直接将所有MATLAB相关的文件赋予权限。

简单粗暴：

    cd ~
    sudo chmod -Rf a+x+w+r /usr/local/MATLAB
    sudo chmod -Rf a+x+w+r .matlab
