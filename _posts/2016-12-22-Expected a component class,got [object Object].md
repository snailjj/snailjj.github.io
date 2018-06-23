---
layout: post
title: "React Native:Expected a component class,got [object Object]"
description: "Expected a component class,got [object Object]"
date: 2016-12-22 15:23:20.000000000 +09:00
tag: ErrorSulotion
---

## 错误描述如下图所示： 

![](/assets/images/post/2016/1222151901.png) 

运行模拟器时会报错：Expected a component class,got [object Object]" 

解决办法如下： 

```Text
代码中出现了小写的组件名，例如：<text>Hello</text> ,改成<Text>Hello</Text>
```