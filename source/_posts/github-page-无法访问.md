---
title: Github Pages 被攻击无法访问
tags:
  - 信息安全
  - 小日志
categories: 小日志
keywords:
  - 信息安全
  - 小日志
description: 2020-3-26 Github Pages 被攻击无法访问
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-27/BogImages/1585239502537.png
abbrlink: 7b49fb01
date: 2020-03-26 16:28:22
---

今天早上就在整理我之前发过的一些文章，在做文章迁移

下午的时候，突然也就显示无法访问了
一开始还以为是我的梯子扑街了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-27/BogImages/1585239532239.png)


然后，更换了一个节点，挂全局去访问
这时候，有意思的画面就出现了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-27/BogImages/1585239933132.png)

不安全？你跟我说不安全？我用的可是github page里面强制使用的证书
直觉告诉我 这次gayhub可能被人家攻击了
然后跑去群里跟小伙伴告知了一声

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-27/BogImages/1585240204931.png)

然后，使用edge打开了一下下我的博客，显示如下

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-27/BogImages/1585239812716.png)

导出证书瞅一眼

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-27/BogImages/1585239842124.png)

帅啊，某位老哥的QQ引人瞩目
这证书劫持可是真滴骚骚了，访问就 不安全警告 或者 就直接访问不了
还有一些大佬们搭建的github page 上面的博客我也打开验证过了，无法打开。

猜猜Github的人什么时候才能修复好？