
--
layout: blog
title: 'MacOS安装Eclipse之后不能运行，不能分享文件'
date: 2017-06-01 12:11:34
categories: blog
tags: code
image: ''
lead-text: 'MacOS下载完Eclipse之后无法直接打开的解决方法'
---

## 在MACOS下安装Eclipse下载后无法打开
```
The Eclipse executable launcher was unable to locate its 
companion shared library.
```
下载Eclipse之后直接点击图标，出现上述错误，在尝试了网上的解决方法：1、修改eclipse.ini；之后发现并不能解决问题。还是打不开，绝望
后来反转了下思路，在eclipse.ini是一些启动参数，可以不用理会，最终打开程序的是在eclipse.ini对应文件夹下面的哪个eclipse文件，通过终端便可以将其打开
右键点击eclipse图标->显示包内容->Contents->MacOS文件夹子里面


