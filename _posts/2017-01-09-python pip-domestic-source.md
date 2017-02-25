---
layout: post
title: Python pip 国内镜像源
categories: [Python]
tags: Python
---

通过pip来安装一些python第三方库的时候，由于那道墙，总会出现ReadTimeoutError错误

翻墙是一个办法

除了翻墙之外，还可以使用国内的镜像源来安装：https://pypi.mirrors.ustc.edu.cn/simple/

以安装numpy为例：

    pip install --index https://pypi.mirrors.ustc.edu.cn/simple/ numpy

其中，--index https://pypi.mirrors.ustc.edu.cn/simple/ 指定了通过这个镜像源来安装
