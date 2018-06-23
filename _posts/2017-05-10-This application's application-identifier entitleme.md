---
layout: post
title: "This application's application-identifier entitlement does not match that of the installed application. These values must match for an upgrade to be allowed"
description: "更换开发者账号进行真机测试时 Bundle Id 也换了"
date: 2017-05-10 13:54:11.000000000 +09:00
tag: iOS
---



# 开发时用的是开发者账号A,然后等到后期用的开发者账号B,并且手机上保留原来的测试App,更改Bundle重新真机测试时,报错
# This application's application-identifier entitlement does not match that of the installed application. These values must match for an upgrade to be allowed

## 解决方法：

## 1、Xcode－Window->Devices
## 2、选中你的设备，在右边的installed Apps中删除这个App
## 3、重新编绎即可

