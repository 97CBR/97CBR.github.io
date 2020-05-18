---
title: VMWare 安装Ubuntu18.04-Live-Server记录
tags:
  - VMWare
  - Ubuntu
keywords:
  - VMWare
  - Ubuntu
categories: 虚拟机
description: VMWare 安装Ubuntu18.04-Live-Server记录
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586508510597.png
abbrlink: 9c1fd75a
date: 2020-04-10 15:07:22
---

## 前言

**我们该如何根据需求来确定我们需要装的系统呢**
那就当然是按照需求来呀，比如我们并不需要界面的Linux，就可以装今天的主角，Ubuntu18.04-Live-Server。当然如果我们需要界面的话，就装带界面的发行版。如果我们打算装系统来做渗透，那么kali欢迎你。

这里我需要装一个Ubuntu来部署一个蜜罐系统，嘿嘿，不用界面就好。所以，选择了这个server版本，那我为啥不用docker呢？哈哈哈

## 将镜像下载回来

这里我们去清华镜像站下载，下载比较快点 [点击进入](https://mirrors.tuna.tsinghua.edu.cn/)

选择发行版

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586493199109.png)

我要18.04的

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586493186710.png)

用种子或者直接iso镜像都行

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586493169488.png)

## 在虚拟机里面安装

**这部分可以参考以前装kali的记录**
创建新的虚拟机

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494359575.png)

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494381222.png)

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494452916.png)

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494592865.png)

## 启动虚拟机进行镜像安装

这里要等一两分钟
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494682620.png)

英文就好
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494756765.png)

不要升级
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494783386.png)

美式键盘布局
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494837988.png)

设置网络
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494847221.png)

不设置代理，这个随意自己
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494873500.png)

改成阿里源，下载快N倍
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586494994282.png)

http://mirrors.aliyun.com/ubuntu/

用整个虚拟磁盘
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495066864.png)

默认磁盘
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495078192.png)

分区，，默认
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495159902.png)

提示要格式化，继续就好
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495171283.png)

设置我们主机的名字
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495367233.png)
ps：wkssd是啥意思呢，有兴趣可以猜猜

这里要用空格**确定勾上安装openssh server**方便后续进行ssh连接
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495351706.png)

要是没有自己要用的软件，就直接选择done安装吧
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495470980.png)

正在安装的亚子
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495494986.png)

安装完会给这个提示，并不会自动重启
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495707011.png)

装完之后，选择reboot会起不来。提示我们要将安装媒介移除，我们要将之前选的ISO镜像去了，选择DVD为自动检测
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495854660.png)

进入登录界面
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495890490.png)

前面可能会连续都登不进去，我瞅了一下回显
感觉是生成秘钥什么的
试了两三次之后，就可以正常登录了
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-10/BogImages/1586495982450.png)

OK，差不多就在这样子，简单的记录一下装这个Ubuntu-live-server~嘿嘿

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>