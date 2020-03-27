---
title: 黑群晖NAS建立空间并映射到电脑中
tags:
  - NAS
  - 有趣的东西
categories: NAS
keywords:
  - 黑群晖
  - NAS
  - 有趣的东西
description: 黑群晖NAS建立空间并映射到电脑中
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557539446182.png
abbrlink: 95274ff5
date: 2019-05-11 00:00:00
---


## 前文再续，书接上一回
上文写到NAS已经装上硬盘和点亮了
有小伙伴问到这个NAS有什么用
那么，NAS有什么用呢？

## 简单介绍一下NAS

>NAS（Network Attached Storage：网络附属存储）按字面简单说就是连接在网络上，具备资料存储功能的装置，因此也称为“网络存储器”。它是一种专用数据存储服务器。它以数据为中心，将存储设备与服务器彻底分离，集中管理数据，从而释放带宽、提高性能、降低总拥有成本、保护投资。
>来源：百度百科

在NAS里面，通常会将硬盘组成RAID来提供服务
那么问题来了？RAID是啥？

## 简单介绍一下RAID

>独立硬盘冗余阵列（RAID, Redundant Array of Independent Disks），旧称廉价磁盘冗余阵列（Redundant Array of Inexpensive Disks），简称磁盘阵列。其基本思想就是把多个相对便宜的硬盘组合起来，成为一个硬盘阵列组，使性能达到甚至超过一个价格昂贵、容量巨大的硬盘。根据选择的版本不同，RAID比单颗硬盘有以下一个或多个方面的好处：增强数据集成度，增强容错功能，增加处理量或容量。另外，磁盘阵列对于计算机来说，看起来就像一个单独的硬盘或逻辑存储单元。分为RAID-0，RAID-1，RAID-5，RAID-6，RAID-7，RAID-01，RAID-10，RAID-50，RAID-60。
简单来说，RAID把多个硬盘组合成为一个逻辑扇区，因此，操作系统只会把它当作一个硬盘。RAID常被用在服务器计算机上，并且常使用完全相同的硬盘作为组合。由于硬盘价格的不断下降与RAID功能更加有效地与主板集成，它也成为普通用户的一个选择，特别是需要大容量存储空间的工作，如：视频与音频制作。
最初的RAID分成不同的等级，每种等级都有其理论上的优缺点，不同的等级在两个目标间获取平衡，分别是增加数据可靠性以及增加存储器（群）读写性能
>资料来源：维基百科

## 去群晖官网计算RAID计算器

在群晖官网我们也可以找到RAID空间计算器

假设我们怼两个1T的和两个500G的盘进去

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557539446182.png)

那么，RAID0和RAID1 的可利用空间如下

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557539434328.png)

RAID 5 和 SHR 的可利用空间如下

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557539504075.png)

这些个 RAID 的分类区别，后面有机会再细写

## 根据需求建立存储池
OK，在此处，我们在建立存储池的时候
根据我们的需求来进行建立
比如，我在这个NAS上面并不打算存一些很关键、很重要的数据
比如一些图片、视频之类的内容
我就不用选那些需要大量空间进行冗余的类型了
所以，此处，选用RADI 0
但是，本人还有一些重要的数据要保存的
比如项目代码、其他的关键的文件
然后，用另外两个硬盘，建立SHR来保存一些重要的数据

建立的时候，长这样：
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557540231628.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557540482720.png)

OK，咱们现在终于是把空间也建立好喽
## 创建文件夹进行映射使用
那就创建一个文件夹供自己的主机映射出来使用吧

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557540552367.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557540639713.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557540596564.png)


创建好之后，可以在文件服务这个选项里面找到访问方式


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557540684850.png)

## WIN10进行网络磁盘映射

那么我们回到PC机里面
进入我的电脑

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557540774004.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557540983596.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS内网穿透访问/1557541089093.png)

然后便可以在自己电脑上面看到多了一个盘啦

## 测试一下速度

咱们测一下传输速度

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS建立空间并映射到电脑中/1557541423163.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-5-11/黑群晖NAS建立空间并映射到电脑中/1557541432545.png)

在局域网内，把我设备支持的最大速度拉满……
要是设备能支持千兆的话，速度肯定还可以再拉高

至此差不多就是完成咱们NAS的基本设置了
能局域网访问、速度拉满
但是它能干的远不止这些
比如，我们可以远程推送下载任务给它，它便可以在后台帮我们把内容下载好
回来便可以使用了
或者创建一个在线相册，去玩、去约会的时候，对吧
拍了照片之后，只要一连上WIFI
就自动上传这些照片、视频到自己的NAS服务器
有了基本拉满的速度，10MB/s甚至某些同学可以有100MB/s
有了这个，请问还用什么某度云，几十K 每秒的速度真是令人很不满意~
当然我们也可以搭建一个在线的音乐平台、视频播放平台什么的
哈哈哈

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>