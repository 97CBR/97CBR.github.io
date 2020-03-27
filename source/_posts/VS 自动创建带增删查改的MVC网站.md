---
title: Visual Studio 2017 自动创建带增删查改的MVC网站
tags:
  - MVC
  - .Net
  - Visual Studio
categories: Visual Studio
keywords:
  - MVC
  - 自动创建
  - Visual Studio
description: 利用 Python + Flask 搭建软件更新服务
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-26/BogImages/1585153493300.png
abbrlink: ca407765
date: 2019-11-10 00:00:00
---

## 妹纸都开口了，还有拒绝的理由吗
某天某妹纸找我，说这个MVC的创建不太会，要记一下controller、models、还有页面引用的东西，好不方便~
记不住咋办嘛~有没快速生成适合自己使用的带有增删查改功能的MVC网站呢
方法是有滴，经过本人的一番摸索~且听我给你一一讲述

步骤如下，咱这么简单的操作就不说这么多了，关键的内容咱再讲一下哈

## 创建Web应用程序项目

咱们先创建一个项目，新建->ASP.NET Web应用程序，起个有意义的名字->MVC->创建即可

![1](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354143420.png)

![2](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354168572.png)

![3](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354184787.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354201510.png)


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354214238.png)

## 进入models文件夹，创建属于我们的类

创建好了项目组之后，咱们二话不说，来到models文件夹
右键，添加一个类，记得类的名字咱起个有意义的好吗

![1](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354242831.png)

![2](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354276507.png)

## 安装EntityFramework
由于类里面要用到EntityFramework，所以，咱们给装一下，如果装了的同学，可以忽略这一部分
### PM安装
![1](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354458639.png)

![2](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354488862.png)

### 界面化安装
另外一个安装方法-by小机灵鬼

![1](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354545782.png)

![2](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354553488.png)

创建好类之后，就可以看到这样子的内容了
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC网站/1573357028594.png)

## 创建好类之后，准备添加属性

这里是关键，大家注意看了哈

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354683024.png)

先导入咱们的Entity，然后在我们的类里面添加我们要加属性，什么ID，名字、性别、邮件什么的，随意大家，请自由发挥
接着就是再写一个派生自DBContext基类的类。用来处理提取、 存储和更新的实体框架我们自己的数据库上下文Marx类在数据库中的实例
public DbSet<Marx> marxes { get; set; }
这样子子就写好了

## 在controllers里面操作
然后再点到controllers，右键添加控制器，选择包含识图的MVC5控制器

![1](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354716778.png)

![2](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354780938.png)

![3](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354804545.png)

![4](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354824972.png)

![5](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354835569.png)


有些同学可能会出这样的报错，咱们重新生成一下解决方案就好了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354844467.png)

![1](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354858584.png)

![2](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354869227.png)

已启动全部重新生成: 项目: MarxMVC, 配置: Debug Any CPU

再配置一次

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354900325.png)
点了添加之后，就会出现这样子的操作界面

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354908752.png)

一会儿过后，就可以看到控制器、页面已经全部生成了
## 已经自动生成控制器、页面

![1](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354950453.png)

![2](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354958268.png)

## 测试一下效果
进入index的页面，点击运行，给大家展示一下 创建、修改、删除、详情的功能

![1](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573354976250.png)
进入首页

![2](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573355031667.png)
使用创建功能

![3](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573355078545.png)
回到首页可以看到生效

![4](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573355088663.png)

![5](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573355107207.png)

![6](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573355114110.png)

![7](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573355183157.png)

![8](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-10/VS_自动创建带增删查改的MVC系统/1573355193810.png)

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>