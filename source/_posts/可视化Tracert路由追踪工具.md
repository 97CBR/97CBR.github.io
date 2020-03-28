---
title: 可视化Tracert路由追踪工具
tags:
  - QT
  - 计算机网络
categories: QT开发
keywords:
  - QT
  - 计算机网络
description: 可视化Tracert路由追踪工具
cover: 'https://marxcbr.oss-cn-shenzhen.aliyuncs.com/Blog/trace.png'
abbrlink: a9897d1a
date: 2019-06-18 00:00:00
---

## 前言

tracert 很多同学可能都没用过，以至于连名字可能都不知道，即使用过可能也只是知道这个Windows自带的命令可以查看路由，而不太清楚这背后的原理。

所以，最近就自己搞了一个工具，可视化Tracert路由追踪工具。方便自己查看网络路由情况。实测效果和Windows自带的tracert一样。并提供多了两三个功能以便查看

- 平均路由时间
- 路由时间方差
- 使用颜色直观反映时间情况

## Windows自带的tracert运行效果

先给大家看一下Windows自带的tracert运行效果。

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-21/可视化Tracert路由追踪工具/1555848632003.png)

## tracert追踪原理

tracert是一款计算机网络工具，可显示数据包在IP网络经过的路由器的IP地址。
为何可以追踪？
原理如下：
>程序是利用增加存活时间（TTL）值来实现其功能的。每当数据包经过一个路由器，其存活时间就会减1。当其存活时间是0时，主机便取消数据包，并发送一个ICMP TTL数据包给原数据包的发出者。
程序发出的首3个数据包TTL值是1，之后3个是2，如此类推，它便得到一连串数据包路径。注意IP不保证每个数据包走的路径都一样。

差不多就和下面这个图一样，每一种颜色代表没一次发送的数据包。绿色是TTL为1的，蓝色为TTL为2的……依次类推……
PS：原谅我的灵魂画作…… 侧面印证了数位板的重要性，有没推荐的啊，朋友们

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-21/可视化Tracert路由追踪工具/1555845935593.png)

## 实现思路

所以，我们只要用ICMP协议发送自己构造的数据包就可以实现路由追踪功能了。即构造TTL值从1开始的ICMP数据包，然后逐个增加TTL值，直到到达目标机器上面。
这里有个注意的点，并不是所有网关都会如实返回 ICMP 超时报文。出于安全性考虑，大多数防火墙以及启用了防火墙功能的路由器缺省配置为不返回各种 ICMP 报文，其余路由器或交换机也可被管理员主动修改配置变为不返回 ICMP 报文。因此 Tracert 程序不一定能拿全所有的沿途网关地址。所以，当某个 TTL 值的数据包得不到响应时，并不能停止这一追踪进程，程序仍然会把 TTL 递增而发出下一个数据包。一直达到默认或用参数指定的追踪限制才结束追踪。

## ICMP报文格式

然后给大家展示一个ICMP报文格式

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-21/可视化Tracert路由追踪工具/1555846553971.png)

这个报文类型，其实有很多种，大家可以看到前面类型一共保留了8位，2的8次方=256。但是现在其实只分了大概40种左右。在这里我们将类型选为 8-请求回显 。

## 使用C++ 和 QT5 进行开发

然后编程语言采用C++，使用winsock发送数据，界面的话，就采用QT5整合界面。

### QT开发的软件打包方法

在这里扯一下QT开发的软件该怎么打包
在这个过程中，我们要选择对应的版本工具进行打包。
比如我这里用的是VS2017 release 的 x64 位的软件 ，所以，在选择打包工具的时候，就应该用对应的版本。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-21/可视化Tracert路由追踪工具/1555847937646.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-21/可视化Tracert路由追踪工具/1555847999791.png)

打开后，cd 到软件目录，输入 QT 打包命令 **windeployqt** 你软件

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-21/可视化tracert路由追踪工具/1555844904194.png)

## 效果
最后，软件跑出来的效果大概长这样子：

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/Blog/trace.png)

由于代码太多，项目不方便在文章中展示
该项目已经上传到GitHub，欢迎大家去fork、给star,GitHub地址[Tracert](https://github.com/97CBR/Tracert)

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>