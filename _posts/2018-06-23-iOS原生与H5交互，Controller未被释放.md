---
layout: post
title: "原生与H5交互，Controller未被释放问题"
description: "iOS开发"
date: 2018-06-23 15:45:11.000000000 +09:00
tag: iOS
---




### 在iOS上加载web页面,同时又需要在web页面上通过js调用iOS的原生功能,比如第三方登录,支付,iOS视图操作等等,这个时候一般都需要用到webView-js互交.本文主要记录开发过程中遇到的使用WKWebView与js互交时容易出现的内存泄漏问题.



1、WKWebView-JS互交开发中经常使用下面这个方法: 

	config.userContentController = WKUserContentController()
    config.userContentController.add(self, name: "GoodGoodStudy,DayDayUp")

2、但是在网页消失，或者回到前一个界面时，理论上VC应该被释放，但是VC没有被释放，下面deinit方法没执行

	deinit {
        self.webView.configuration.userContentController.removeScriptMessageHandler(forName: "GoodGoodStudy,DayDayUp")
    }

3、原因：大概是循环引用的问题吧

	VC强引用WebView，webView强引用configuration,WKUserContentController对象的addScriptMessageHandler方法的scriptMessageHandler参数传入了将控制器本身(猜测addScriptMessageHandler将会对scriptMessageHandler参数传入的对象做强引用），可能是造成了循环引用的问题

4、解决办法暂时有两种，亲测有效
	
	a、在viewDidDisappear中removeScriptMessageHandler
		有效，但是不同场景下，这个办法不实用，在push新VC时，也被移除掉了，从而回到网页VC时，与JS不会再交互，重新注册也不好使

	b、设置一个中间层来，来实现addScriptMessageHandler方法对传入参数弱引用，达到消除循环引用的问题,具体代码如下2图，具体的也就不说了

![image](/assets/images/post/2018/06/20180623163825.png)

![image](/assets/images/post/2018/06/20180623163843.png)


