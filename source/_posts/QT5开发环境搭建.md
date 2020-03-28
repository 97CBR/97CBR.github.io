---
title: Visual Studio 2017搭建QT5开发环境
tags:
  - QT
  - C++
  - 环境搭建
categories: QT开发
keywords:
  - QT5开发环境搭建
  - QT
description: Visual Studio 2017搭建QT5开发环境
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-26/BogImages/1585153085793.png
abbrlink: '58696240'
date: 2019-05-30 00:00:00
---


## 工欲善其事，必先利其器

之前有说过要写QT开发的内容的，这不就写一个在C++下开发QT的文章啦
还是原来的调调，工欲善其事，必先利其器
咱们还是从最基础的环境的搭建上面说起

在这里，本人使用的是visual studio 2017 作为开发软件

所以，大家可以去将VS装起来再进行后面的步骤
现在VS2019都出来了，界面更帅了、语法提示似乎也更智能了，我都心动了，看什么时候就装一个VS2019耍耍

好的，如果之前还没装VS的同学看到这里差不多也还是没装的，装了的同学请忽略这句~
## 安装QT扩展
装好VS的同学可以在
“**工具”-“扩展和更新”-“联机”-“搜索”-“qt”**
里面找到 vs关于qt开发的插件
下载之后，关掉VS你就会看到安装的提示了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559220338898.png)

全文完~

那岂不是很水？我自己都无法接受，哈哈哈

不不不，咱们还要将QT装好到电脑上才可以的~，这一个插件就能搞定QT的开发环境搭建，想的美哈

## 安装一下QT

这里有个网站，可以直接到他们网站[下载](http://www.qtcn.org/bbs/read-htm-tid-1075.html)
现在已经提供5.12.2的版本下载了，3.9个G，说大不大，说小也不小了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559220562231.png)

好的，如果你现在用的电脑看的本文，可以先转去下载QT了，3.9G，要下一段时间呢

emmm，如果之前还没装QT的同学看到这里差不多也还是没装的，装了的同学请忽略这句~
那，咱们先继续看文章好吗？看完看懂再搞，一步搞定不踩坑。

我这里用的是5.11.2，打开它，可以看到这样的界面

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559222098705.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559222167601.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559222249182.png)
如果想开发的程序支持32位，上面的那个32-bit 必选好吧
其他的那些，对应的意思，相信大家也能看懂，我就不做更多的解释了

然后选择好几个选项之后，会叫你选择许可协议~
这个看大家的需求，LGPL 和 GPL 是有区别的，请自行百度谷歌看这两个协议的区别哈
我就不做扩展了
接着就下一步，可以看到安装的情况

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559222588652.png)

好的，等大概半个钟左右，QT便安装完了

## 回到VS中配置扩展插件

这个时候，咱们回到VS 里面
刚才已经安装好 QT 的插件了，可以看到一个选项

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559225032512.png)

然后，选择 option-add 添加

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559225057540.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559225077852.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559225251851.png)

添加的时候，选到自己的安装目录，对应的几个类型里面就好了
添加完之后，可以选择默认的qt版本

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559225310531.png)

OK，差不多已经走到最后一步了
## 创建一个项目测试一下是否正常

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559225398564.png)

在这里勾上自己待会可能要用上的库，即使在这里不勾上，后面如果要用到的话，其实可以可以后面再添加的。不过就相对麻烦一点点，所以大家在这里尽量还是选好自己要用上的库吧

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559225418029.png)

然后再这里的话，大家就可以选择自动创建的类的名字，相对随意。不过我看到很多大佬都是将class的名字改成MainWindows，大家可以参考一下下。可以选上预编译头和默认图标和小写的文件名的可选项。我一般都会默认选上这个图标、原生的图标其实还真挺好看的，起码比默认的那个灰灰的没图标要帅~

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559225514353.png)

Finish之后，就可以看到项目的概况了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559225749500.png)

>ui是画界面，做界面设计用的
qttest.h是自动生成的头
.qrc是用来编辑添加删除资源的
main.cpp，这个写过C语言的，大家都懂哈
qttest.cpp则是用来setup这个界面的，我们可以在这里面写一些逻辑代码、功能函数啥的

双击ui文件，就又可以看到之前那篇PYQT环境搭建的那个熟悉的designer界面了
又是随意添加一些控件进去

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559226137300.png)

Ctrl + S保存之后，就不用管它了
回到VS里面，直接F5,启动！

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-30/QT5开发环境搭建/1559226288039.png)

没错，就这么简单，他就跑起来了~
比之前的那篇PYQT的开发环境搭建还要简单有没有，有没有！
只要装东西就完了~

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>