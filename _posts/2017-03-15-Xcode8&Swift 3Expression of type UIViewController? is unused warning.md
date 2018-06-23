---
layout: post
title: "Xcode 8, Swift 3: “Expression of type UIViewController? is unused” warning"
description: "popViewController 和 popToRootViewController 是有返回值的哟。"
date: 2017-03-15 15:06:11.000000000 +09:00
tag: iOS
---


### Swift3大的改动点之一就是所有带有返回值的函数 没有变量去接收返回值时 ，就会报warning 对于像我这种强迫症患者，看起来好烦呐

#### popViewController和popToRootViewController基本上人人都用，但是有多少人知道这两个函数其实是有返回值的。其实我也不知道。。。。gg

```
// Pops view controllers until the one specified is on top. Returns the popped controllers.  

open func popViewController(animated: Bool) -> UIViewController? // Returns the popped controller.  

open func popToViewController(_ viewController: UIViewController, animated: Bool) -> [UIViewController]?


##### 知道这个原因 然后 解决办法就有了啊

_ = self.navigationController?.popViewController(animated : true)
```


