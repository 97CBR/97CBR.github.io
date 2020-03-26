---
title: 用QSS给QT默认的控件穿件衣服
tags:
  - QT
  - QSS
  - 开发技巧
categories: QT开发
keywords:
  - QT
  - QSS
  - QSS界面美化
description: 黑苹果装机记录
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-26/BogImages/1585153085793.png
abbrlink: 479bb35e
date: 2019-06-18 00:00:00
---

## 前文再续，书接上一回

继续上面的那个磁盘映射软件，咱们上一篇已经成功利用Windows中C++的一个接口，实现了网络驱动器映射的功能

但是，长这样的软件，您想用吗？？？

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560865577695.png)

不啊，咱们看到这个的软件，相信大家都会毫不犹豫的关掉
可能还会吐槽一下，这~软件也太丑了吧。。。
毕竟老话有讲：人靠衣装，佛靠金装
所以，咱们今天，大概说说，怎么搞，将自己的控件穿件衣服
让咱们自己写的软件变的美美哒

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560865778310.png)

大概我随意给它变成了这样子

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/效果.gif)

那，咱们就给自己的那个软件来穿件衣服吧

## 删除不需要的控件

删除不需要的默认生成的控件
因为我们的软件并不需要默认的菜单栏
也不需要默认的状态栏
所以，咱们选择删除它
如下所示

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-15/给QT默认的控件穿件衣服/1560603257772.png)

OK，删完了咱们不要的东西
## 给底面换个背景颜色

咱们准备给最里面的底面换个颜色
大概就像，女生给自己打个粉底吧

步骤就像下面这样

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-15/给QT默认的控件穿件衣服/1560603363845.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-15/给QT默认的控件穿件衣服/1560603389608.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-15/给QT默认的控件穿件衣服/1560603412558.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-15/给QT默认的控件穿件衣服/1560603439991.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560866088683.png)

怎么样，左边是效果图，右边是原图
“美白效果”是不是满分啊
当然，咱们也可以把颜色换成自己喜欢的任何一个 RGB 值

## 添加自己的组件

添加自己的控件或者是利用控件组合起来的控件
大概长这样

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560866491488.png)

当然，这个样子也是用默认控件做成的
摆起来仍不美观
但是，让自己这个组合控件变的更好看并不是咱们今天的主要话题

我们这次，先给大家讲一下组合控件该怎么控制、怎么摆
上面那块东西，用了这几个控件
主要考虑用来做驱动器的状态显示：
图标、本地盘符名称、远程路径、已用容量、最下面是显示已用和总空间

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560866696566.png)

我们按照设想的样子给他摆好之后
注意哦，我用的并不是Widget、而是QH\VBoxLayout 这两个做容器
后期，QH\VBoxLayout 可以转换到widget的，不用担心
因为，控件的默认sizepolicy有这几个参数

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560866943372.png)

默认的控件，垂直策略都是 Fixed
咱们可以根据自己的需求，而改变自己的策略

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560867025841.png)

这几个策略的区别在这里，我列出来给大家看看

Fixed<<<>>>缺省大小是唯一可以接收的改变，因此部件不能放大也不能缩小。
Minimum<<<>>>缺省大小是最小值，并且是充分的。部件允许扩展，但是并不倾向扩展（例如：水平方向上的按钮），不能比缺省大小提供的大小更小。
Maximum<<<>>>缺省大小是最大值，假如其它部件需要空间并且不会破坏该部件，那么该部件允许被缩小（例如：一个分割线）。
Preferred<<<>>>缺省大小是最佳效果，部件允许放大或缩小，但不倾向于扩展比sizeHint()大（QWidget的缺省策略）。
Expanding<<<>>>缺省大小是合理的大小，但部件允许缩小并且可用。部件可以利用额外的空间，因此它将会得到尽可能多的空间（例如：水平方向上的滑块）。
MinimumExpanding<<<>>>缺省大小是最小值，并且是足够的。部件允许使用额外空间，因此它将会得到尽可能多的空间（例如：水平方向上的滑块）。
Ignored<<<>>>缺省大小将会被忽略，部件将会得到尽可能多的空间。

然后，我们到QVBoxLayout这个控件中
我们可以对QVBoxLayout这个容器中的控件按照比例进行排序
如下所示：

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-15/给QT默认的控件穿件衣服/1560604473422.png)

你看，箭头指的 1,1,1 分别就是 horizontallayout_2、label、progressbar 这三个东西的比例了
而layoutSpacing这个会控制控件间隔

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560867540871.png)

左边的是设为10，右边的设为0
区别还是挺大的哈

OK，到这里，我们成功搞出了一个组合控件

## 给按钮换个样子

大概可以做成这样子

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560869173668.png)

咱们给他的字换成了白色、底为红色、边界是圆润的
大家并不用盘它哈
做到这个效果也很简单
如上文一样，咱们编写QSS
如下所示

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560869275715.png)

QSS和CSS很像
Qt支持所有的CSS2定义的选择器
大家看着CSS怎么写，大概也可以试试，万一就通用呢


## 做一下动效

我们处理一下上面文章刚开头看到的那个界面滑动的效果
这个动效的实现，也很简单

我们大概可以理解成，将界面分成3块
然后，点击按钮的时候，将下面的那块平滑移动覆盖到上面中间那块
当然，如果是已经覆盖了，就将他下滑而将中间显示出来
这里要用到三个函数
分别是
setGeometry：设置控件大小及位置
resize：设置界面大小
repaint：刷新界面

在这里，说一下，这个动效的实现是要用循环来完成的
咱们上面三个函数都得放到循环里面哦

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-18/用QSS给QT默认的控件穿件衣服/1560868559094.png)


<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>