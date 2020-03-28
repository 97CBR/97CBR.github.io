---
title: PYQT界面之按钮绑定使用
tags:
  - QT
  - Python
  - null
categories: QT开发
keywords:
  - PYQT
  - 按钮绑定
description: PYQT界面之按钮绑定使用
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-26/BogImages/1585153085793.png
abbrlink: 6b9be86d
date: 2019-5-7 00:00:00
---

## 前言
今天就继续写一下QT界面开发的东西
之前好不容易才将PYQT的开发环境搭建出来
总不能搭好就不了用吧

所以，我们今天就写一下**按钮怎么绑定到函数**
平时，我们使用软件的时候，会有很多的按钮
点击后，就会有不同的功能是吧
然后，写代码的同学都知道，功能都得写在函数里面
但是，我们在designer里面设计界面的时候
没办法直接在里面吧功能给写好并绑定的
所以，我们就得自己动手解决这个问题


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-7/PYQT界面之按钮绑定使用/1557209047622.png)

## 准备动手开始

OK，假设我们已经设计好一个界面了
长这样：

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-7/PYQT界面之按钮绑定使用/1557209167445.png)

眼尖的同学肯定知道我又偷懒了
没错，这个界面是之前写过的那个软件更新系列里面用到的
没看过的同学，可以回我之前发的文章里面找来看看哈

然后，可以看到有两个按钮
一个是“检测更新”
一个是“下载”

然后，pyuic之后，再继承生成的界面类
就可以在里面写自己的函数了

在那篇文章里面我写了两个函数
showUpdate、updateNow

我们该怎么绑定到按钮

``` python
self.pushButton.clicked.connect(self.showUpdate)
self.pushButton_2.clicked.connect(self.updateNow)
```

就在这样就搞定了
是不是不敢相信，其实就是这么简单

可能会有些同学需要到函数传参
这个时候，我们可以有两种办法

``` python
self.pushButton.clicked.connect(partial(self.showUpdate, 1111))
self.pushButton.clicked.connect(lambda: self.showUpdate(2222))
#函数：
def showUpdate(self,num)

```

这两种办法都可以实现我们的传参需求哦

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>