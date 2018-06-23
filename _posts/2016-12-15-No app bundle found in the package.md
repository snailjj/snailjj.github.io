---
layout: post
title: "No app bundle found in the package"
date: 2016-12-15 
description: "ERROR ITMS-90167:No app bundle found in the package"
tag: ErrorSulotion
---   


## ERROR ITMS-90167:No .app bundle found in the package


### 由于mac系统升级到了10.12.1,iOS项目是在Xcode7.x上编写及打包，用xcode7.x自带的Application loader上传遇到了问题
###	然后通过网上查找，最直接有效的方法就是用Xcode8.x自带的Application loader上传.ipa

Application loader打开如下图所示（Open Developer Tool）  
![](/assets/images/post/2016/1215141827.png)


