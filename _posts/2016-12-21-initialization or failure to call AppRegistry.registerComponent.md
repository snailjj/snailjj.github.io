---
layout: post
title: "React Native:error during initialization or failure to call AppRegistry.registerComponent"
date: 2016-12-21 11:33:33.000000000 +09:00
description: "ERROR ITMS-90167:No app bundle found in the package"
tag: ErrorSulotion
---   


## 错误

今天在学习React Native的时候，出现一个问题，我代码检查了一遍，写的没有问题，然后让同事帮忙看了一下，到处没有问题，
但是在终端执行react-native run-ios 的时候，iOS模拟器提示错误 

Application ScrollViewL has not been registered. This is either due to a require() error during initialization or failure to call AppRegistry.registerComponent. 

百度到答案了，然后记一笔： 
第一种原因：可能是真的是你注册的时候 写错了。 

第二种原因：也就是其他情况，可能是端口被占用了 

`切换到项目所在目录，输入react-native start 如果出现Packager can't listen on port 8081那说明端口被占用了。` 

`根据命令行提示进行操作: 

1.lsof -n -i4TCP:8081    列出被占用的端口列表 

2.kill -9 <PID>    找到与之对应的PID然后删除即可 

3.重新运行项目 react-native run-ios/android`


