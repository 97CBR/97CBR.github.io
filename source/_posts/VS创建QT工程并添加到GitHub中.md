---
title: Visual Studio 创建QT工程并添加到GitHub中
tags:
  - QT
  - Visual Studio
  - Github
categories: QT开发
keywords:
  - QT
  - Visual Studio
  - GitHub
description: 利用C++和QT在VS2017里面自己写一个映射网络驱动器
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-26/BogImages/1585153493300.png
abbrlink: 687d52f8
date: 2019-06-09 00:00:00
---

## 怎么在VS中创建一个QT项目并且放到GitHub中呢

怎么在VS中创建一个QT项目并且放到GitHub中呢
因为GitHub或者码云Gitee都是一个很好的提供代码托管的地方对吧
将自己的项目开源到上面，如果项目很好的话，肯定能收获到很多小星星对吧。不仅仅能与全世界的开源爱好者有交流的机会，也会是自己个人展示自己的好平台啊，对吧。

## 闲话少说，操作一波

打开VS，我这里是VS2017，使用其它版本的同学应该也差不多，参考参考借鉴借鉴就好

还记得之前文章里面有给大家说过给VS安装上 QT插件、GitHub插件、Gitee插件嘛

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/VS创建QT工程并添加到GitHub中/1560080663232.png)

如果还没装的话，可以先进去装一下，装好再进行下面的步骤哟

给你两分钟，打开VS然后将插件装起来


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/VS创建QT工程并添加到GitHub中/1560080829083.png)

两分钟过去了~~

## 使用团队资源管理器

我们打开 右边的界面方案资源管理器
然后点击隔壁的 团队资源管理器

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560078136589.png)

这个时候，如果你是装好了这两个插件的话，是应该看到有GitHub和gitee的
### 登录Git平台账号
然后，点击 登录 就可以看到这样的界面了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560078025140.png)

OK，输入好自己的账号密码之后，登录一下下
你就会看到这个GitHub的选项卡下有几个选项了
### 创建仓库
咱们创建一个仓库先

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560078157492.png)

我们在这里将 名称 写一下、描述写一下、路径选好选准确了！
GitHub忽略的选项，他能识别到VS项目的
而许可证的话，看自己情况选
专用存储库是私有的，别人访问不了的。敏感项目的话，建议选上！

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560079685453.png)

我在这里就不点专用了，这里选好代码仓库位置，待会创建项目的时候，你就会发现项目创建路径会自动默认到这个位置了。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/VS创建QT工程并添加到GitHub中/1560081516092.png)

在项目下方的解决方案那里，点击新建~，不是在整个VS左上角点新建哦。看准了哈~

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560078965838.png)

呐，是不是明显看到位置自动是我们刚创建项目仓库的位置啦~

接着就是QT创建项目的常规步骤
## 创建QT项目

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560075844053.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560075927392.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560075964876.png)

因为之前写完那篇环境搭建的文章出来后，有同学给我反映说不够详细，我在这里就完全又走了一次流程了哦。一个截图都不少的~

我们F5直接运行一波，看到能正常出来了。OK，咱们可以先把这个最最最基础的版本给提交到GitHub上去先。后面再提交新的更改进去

点击 **更改**

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560079924063.png)
可以看到项目内的文件的更改情况，添加、删除、修改都可以看到
我们再将这次上传的 更改说明写好，写清楚！

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560080000441.png)
写完之后，点击 **同步**

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/VS创建QT工程并添加到GitHub中/1560082065469.png)

点击 **推送**

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560080029467.png)

看到这样的界面，说明你已经成功将代码更改推送到GitHub上了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-6-9/QT做一个映射网络驱动器的小工具/1560080065247.png)

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>