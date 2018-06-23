---
layout: post
title: "ReactNative基础篇之初识"
date: 2016-12-12 
description: "ReactNative学习"
tag: ReactNative
---   


## 附上[Cocoapod官网](https://cocoapods.org)


### 打开终端，切换Ruby的软件源

> * mac自带的https://rubygems.org 是用的是亚马逊的云服务，需要翻墙。所以放弃移除掉
> * 切换国内的https://ruby.taobao.org/，这个很卡啊，劳资也放弃他了
> * Ruby China社区专注的https://gems.ruby-china.org/ 还是比较人性化的。感谢感谢

#### 对应的操作指令如下:

> * gem sources -l    查看buby的软件源
> * gem sources --remove https://rubygems.org/ 移除掉默认的源
> * gem sources -a https://gems.ruby-china.org/. 添加软件源
> * gem sources -l    查看buby的软件源

### 开始安装cocoapods
其实就是执行 sudo gem install cocoapods 这么简单的事儿，不过 中间会有很多坑。心累～
打开控制台
> * sudo gem install cocoapods 安装cocoapods
> * 执行后等待，有可能会出现下列几种情况


