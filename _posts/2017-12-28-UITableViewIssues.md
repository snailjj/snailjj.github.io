---
layout: post
title: "UITableView遇到的问题"
description: "iOS学习"
date: 2017-12-28 09:22:11.000000000 +09:00
tag: iOS
---


### UITableView 开发过程中遇到的问题


#### 记录一下咯~~



1、当为UITableView.tableHeaderView 赋值的时候 
	
	一定要先加一个aView,然后把xib创建的bView，aView.addSubView(bView),然后UITableView.tableHeaderView = aView。
	如果直接把UITableView.tableHeaderView = bView,约束会没有效果，高度会不受控制的

2、当用同一个UITableView时，要切换不同的Cell，reloadData时，UITableView会flickering   [解决方法在这里](https://stackoverflow.com/questions/15196927/reload-uitableview-with-new-data-caused-flickering)
    
    emmm，在网上找了一个方法试了一下，就是在reloadData()的时候，UITableView.begainUpdates(),然后UITableView.endUpdates(),效果会好很多。
    
3、关于取消UITableView的头粘滞性，需要设置TableView.style = .group,然后进行分组操作，有的TableView.style = .plain,然后进行分组操作时，头会停留在直至该组完全消失
    
  
    
