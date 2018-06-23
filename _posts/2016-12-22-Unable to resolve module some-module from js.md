---
layout: post
title: "Unable to resolve module some-module from /Users/username/projectname/AwesomeProject/index.js: Invalid directory /Users/node_modules/some-module"
description: "Unable to resolve module some-module from "
date: 2016-12-22 17:41:20.000000000 +09:00
tag: ErrorSulotion
---

## 错误描述如下： 

新建一个项目后，把旧项目的文件直接copy到新项目里，然后终端输入react-native run-ios时报错 

```
大概是：Unable to resolve module some-module from /Users/username/projectname/AwesomeProject/iamge.js: Invalid directory /Users/node_modules/some-module
```

解决办法如下： 

```Text
* 1、进入项目根目录，删除node_modules文件夹，然后在当前文件夹下，在终端输入 npm install
* 2、重置包的缓存，终端执行node_modules/react-native/packager/packager.sh --reset-cache
* 3、清理watchman---终端执行watchman watch-del-all
* 4、重新运行项目
```
[具体解决办法点击进入](https://github.com/facebook/react-native/issues/4968) 