---
layout: post
title: Github使用向导
categories: [Tools]
tags: Github
---

我的Github仓库：

    HTTP: https://github.com/mrzhu-cool/

    SSH: git@github.com:mrzhu-cool/

1、新建一个空的工程仓库： SeetaFaceCool

2、在本地终端使用命令行上传本地代码、控制版本：

    echo "# SeetaFaceCool" >> README.md
    git init
    git add README.md
    git commit -m "first commit"
    git remote add origin git@github.com:mrzhu-cool/SeetaFaceCool.git
    git push -u origin +master
