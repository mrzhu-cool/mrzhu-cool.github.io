---
layout: post
title: Ubuntu系统下扩展显示屏
categories: [Ubuntu]
tags: Ubuntu
---

1、假设有两个显示屏，一个插在显卡的DVI接口，一个插在显卡的VGA接口，配置无误

2、在命令行输入xrandr命令，查看所用接口的名称，如：DVI-D-0、VGA-0。（这时候，电脑双屏应该都能显示了，但是模式是xrandr的默认模式）

3、修改显示模式：

克隆模式：

    xrandr --output VGA-0 --same-as DVI-D-0 --auto

右侧扩展：

    xrandr --output VGA-0 --right-of DVI-D-0 --auto

只用主显示器

    xrandr --output VGA --off --output LVDS --auto

只用扩展显示器

    xrandr --output VGA --auto --output LVDS --off
