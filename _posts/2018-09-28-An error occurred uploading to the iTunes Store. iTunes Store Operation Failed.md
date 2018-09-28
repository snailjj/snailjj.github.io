---
layout: post
title: "Xcode9:An error occurred uploading to the iTunes Store. iTunes Store Operation Failed"
description: "iOSbug"
date: 2018-09-28 14:22:11.000000000 +09:00
tag: iOS
---


### Archive Upload to App Store 时报错


![image](/assets/images/post/2018/09/20180928150022.png)


  解决方法：

	打开终端输入

	1、cd ~
	2、mv .itmstransporter/ .old_itmstransporter/

	执行完后，即可upload


[原地址https://stackoverflow.com/questions/46463778/xcode-9-an-error-occurred-uploading-to-the-itunes-store](https://stackoverflow.com/questions/46463778/xcode-9-an-error-occurred-uploading-to-the-itunes-store)
