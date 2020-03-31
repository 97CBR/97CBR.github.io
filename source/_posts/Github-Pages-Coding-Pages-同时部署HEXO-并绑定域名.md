---
title: Github Pages + Coding Pages 同时部署 HEXO 并绑定自定义域名
tags:
  - HEXO
  - 网页优化
categories: HEXO
keywords:
  - Coding Pages
  - 绑定自定义域名
  - HEXO
description: Github Pages + Coding Pages 同时部署 HEXO 并绑定自定义域名
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585416328342.png
abbrlink: dbd2cb4b
date: 2020-03-29 00:32:29
---

## 前言

这篇文章主要介绍一下怎么将HEXO部署到 coding 中 ，并绑定自定义域名
这样子可以有效解决
- 百度无法索引部署在github pages中的博客内容
- 国内访问速度较慢

## 注册 CODING 账号进行部署

先注册一个账号 [CODING](https://coding.net/)

然后登陆进去

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585413955591.png)

### 点击全部项目

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414009129.png)

### 新建项目

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414039555.png)

### 选择 DecOps 项目

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414072691.png)

### 填写项目名称

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414133750.png)

### 进入项目

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414265785.png)

### 点击构建与部署中的静态网站

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414216950.png)

### 进行实名认证

### 新建静态网站

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414380516.png)

添加之后，可以看到这样的界面

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414480421.png)

点击右上角的**设置**

### 绑定自定义域名

先将自己的自定义域名解析到CODING pages 上面先

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585415697355.png)

等到十分钟左右，当然也不固定啦，有时快有时慢~

然后将自己的自定义域名添加到coding pages里面去

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414647414.png)

点击确定之后

可以看到这样的界面，会告诉你域名的证书正在申请中

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585414680605.png)

等几分钟就会告诉你已经申请成功，显示**正常**了

## 将主机的SSH公钥放到配置文件里面去

进入个人设置里的SSH公钥设置

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585415032358.png)

点击 新增公钥
然后将 ~/.ssh/id_rsa.pub 的内容拷进去就OK


## 在 _config.yml 中添加部署配置

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585415202132.png)

``` yaml
deploy:
- type: git
  repository:
  - git@github.com:97CBR/97CBR.github.io.git
  - git@e.coding.net:MarxCBR/marxcbr/marxcbr.git
  branch: master
```

## 执行 hexo d 进行部署

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585415333067.png)

## 踩坑记录

这里的东西才是给力的东西呀，坑不踩无以做记录

### CODING 域名证书申请失败

报错如下
>acme:error:unauthorized: Invalid response from http://www.marxcbr.cn/.well-known/acme-challenge/Gh_xL4eBlBO8RGPnjJ-O7ao7lnFFfJGX4BajL0kE9XE [185.199.111.153]: "<!DOCTYPE html>\n<html>\n <head>\n <meta http-equiv=\"Content-type\" content=\"tex

这里是因为我之前已经部署在github上面启用HTTPS了。所以他检测申请的时候，就会存在问题。
所以，解决方案就是在 域名解析控制台中停用 github pages 的解析。待这边申请成功之后，再进行启用 github pages 的解析

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585415613889.png)

再大概等一会儿，再去coding里面绑定自己的域名
如果急的话，可以用 [站长工具去测试](http://tool.chinaz.com/dns/?type=1&host=www.marxcbr.cn&ip=) 看到github的解析消失之后，就可以去coding申请了

### 设置域名的解析分流

在域名解析控制台中设置域名的解析路线
大概可以像我这样设置，将github的优先解析到国外

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-29/BogImages/1585415979387.png)

## 结语

大概就写这么多了，我明天再具体测测访问速度的变化以及百度这扑街引擎能否检索到我的博客。三更半夜了，睡了睡了~

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>