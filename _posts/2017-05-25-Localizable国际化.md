---
layout: post
title: "iOS项目的国际化"
description: "国际化"
date: 2017-05-25 10:54:11.000000000 +09:00
tag: iOS
---

## 现在一些大型App，当iPhone设置语言为英文时，App的中文也会变成相应的英文，是不是感到很奇妙，就如下面两个图

![image](/assets/images/post/2017/05/20170525171100.png)

![image](/assets/images/post/2017/05/20170525171115.png)

## 其实这个是很轻松很容易就办到的，不用写太多的代码

- 在Localizations里面加上简体中文，如下图

![image](/assets/images/post/2017/05/20170525142912.png)

- 然后新建个文件 随便起名

![image](/assets/images/post/2017/05/20170525142930.png)

![iamge](/assets/images/post/2017/05/20170525142945.png)

- 然后勾选上这两个按钮 支持中文和英文，这样文件就成了两个

![image](/assets/images/post/2017/05/20170525142955.png)

- 比如在中文的文件里写上

![image](/assets/images/post/2017/05/20170525143015.png)
- 英文的文件上写上

![image](/assets/images/post/2017/05/20170525171415.png)

```
然后在模拟器上运行，当你切换手机语言时，会看到相应的App的名字会根据手机语言的切换而显示不同的名字
```

### 重复上面的步骤 新建文件名叫Localizable.strings

![image](/assets/images/post/2017/05/20170527131330.png)

![image](/assets/images/post/2017/05/20170527131340.png)

![image](/assets/images/post/2017/05/20170527131355.png)

#### 当切换iPhone手机语言时 会看到不同语言上面显示的不同的文字