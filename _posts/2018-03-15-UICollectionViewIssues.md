---
layout: post
title: "UITCollectionView遇到的问题"
description: "iOS学习"
date: 2018-03-15 23:05:11.000000000 +09:00
tag: iOS
---


### UITCollectionView 开发过程中遇到的问题


#### 记录一下咯~~



1、当某一小块的View需要把CollectionView显示完整的话，只要把CollectionView的高度设置以下代码就好了
	
	    collectionViewHeight.constant = collectionView.collectionViewLayout.collectionViewContentSize.height

    
  
    