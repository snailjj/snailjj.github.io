---
layout: post
title: "mac执行git命令出现xcrun: error: invalid active developer path解决方法"
description: "iOSbug"
date: 2018-09-26 10:22:11.000000000 +09:00
tag: iOS
---


### mac执行git命令时候出现：

xcrun: error: invalid active developer path (/Library/Developer/CommandLineTools), missing xcrun at: /Library/Developer/CommandLineTools/usr/bin/xcrun


  解决方法：

	打开终端输入

	xcode-select --install

	回车后，系统弹出下载xcode，点击确认，下载完成后即可。（实际上不是下载xcode，可能下载xcode有关插件，下载时长约1分钟）


[原地址](https://blog.csdn.net/kedongjun/article/details/51470506)
