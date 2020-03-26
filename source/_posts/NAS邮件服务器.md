---
title: NAS邮件服务器
tags:
  - NAS
  - 工具
  - 有趣的玩意
categories: NAS
keywords:
  - NAS
  - 邮件服务器
  - 有趣的玩意
description: 在NAS里面搭建起属于我们自己的邮箱
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-26/BogImages/1585214641951.png
abbrlink: fd5228cc
date: 2019-10-21 00:00:00
---

## 为了装13而搭建一个邮件服务器

嘿，各位同学们，看过我之前写过的文章的同学，应该知道，我写了两三篇关于NAS的文章。大部分是关于NAS的内网穿透使用，以及服务搭建的内容。但是，我的那个黑群晖的CPU以及内存的平均利用率才占个百分几和百分之二十左右。
完完全全可以继续挖掘出更加多有趣有意思的服务进行一个使用啊，仅仅当作一个存储阵列，未免有点大才小用了叭~
所以，后续，本人又给他安装搭建起了，Plex Media Serve 、 Docker 、Cloud Sync 、 MailPlus Server 之类的服务。

咱们这期就挑 MailPlus Server 来给大家说说，咱们，怎么去搭建一个属于自己的邮件服务器，对自己的邮箱内容做到一个完全可控。
因为，怎么说，咱们平时用的无论是163邮箱、QQ邮箱、Gmail邮箱、企业邮箱 等等邮箱，咱们不能说是完全归自己管的吧。有些邮件被拦截了，丢失了，咱也不好打电话给客服找回来吧~
要是自己搭建的服务，邮件的发送，接收，数据导入导出，完全都是可以看到的，可控滴呀~
还有咱们给别人留邮箱地址的时候，留的是自己搭建的邮箱，是不是瞬间显得高大上一些呀~

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571654314451.png)

说一下我用到的东西：

 - 一部NAS，咱们现在用的是黑群晖
 - 一个域名，咱用之前自己注册过的
 - 一部拥有公网IP的服务器


OK，咱们现在开始操作好吗~

## 进入群晖控制面板进行安装软件

先把咱们的群晖界面打开，进入控制面板，如图所示
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-16/NAS邮件服务器/1571225285844.png)
把上面指的两个套件（MailPlus、MialPlus Server）安装好，过程如下

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-16/NAS邮件服务器/1571225300770.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-16/NAS邮件服务器/1571225319869.png)
转好之后，咱们就要打开 **MailPlus Server**

### 打开 MailPlus Server

选择创建新邮件系统

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-16/NAS邮件服务器/1571225856682.png)

配置一下这个页面，咱们选择好网络界面用的是那个网卡，一般的同学都是一个的吧~也有可能有大佬有两三个。咱们自己选好就可以了。其实个人感觉这个问题不大。主要的是域名还有主机名的配置，填对。
域名就填自己的域名，不要www，直接填域名。主机名就用mail 做前缀吧~
如下所示

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-16/NAS邮件服务器/1571225903553.png)
过些许时间~

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-16/NAS邮件服务器/1571225963356.png)
然后，打开一下，**MailPlus Server**
进入**账号**，将自己想要激活的账号激活了，我这里选择把 admin 激活
记得，勾上之后，要点**应用**呀，带哥们~


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-16/NAS邮件服务器/1571226020897.png)

切换选项卡到服务，把几个功能都检查勾选一下，如下所示

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571655765861.png)

### 打开Synology应用程序门户进行设置

下面打开**控制面板**，打开**Synology应用程序门户**，可以看到如下界面

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-16/NAS邮件服务器/1571226056643.png)

点一下**MailPlus**，然后再点**编辑**


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-16/NAS邮件服务器/1571226080767.png)

点击**启用自定义别名**，在别名处，填入咱们设的mail
然后勾上 **启动自定义域**，然后填入咱们的www前缀的域名，顺手把 HTTP/2 启用一下

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571655352670.png)

然后，鉴于之前写过了NAS内网穿透的内容。咱们就不多重复搭建内网穿透的过程了。详见：远程访问NAS内网穿透
直接说一下，我是怎么将NAS绑定到我公网IP的服务器以及设置的域名解析的哈

## 旧文内容更正声明

这里更正一下之前那篇 远程访问NAS内网穿透 文章，里面介绍端口转发的办法是使用 **rinetd** 这个工具来快速实现。
然后，实践之后，现实给了我**狠狠的两拳**

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571656268500.png)

原因就是，某天我收到了服务器监控脚本发到手机的邮件，说CPU占用率超高，达到90%+了。然后，赶紧上线查了一下~我的一眼就能看到我的CPU被某进程疯狂蹂躏~它就是rinetd。
我觉着CPU当时的内心想法是这样的：

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571656493685.png)

具体原因是因为：rinetd使用的是select、并不是epoll。

解决办法就是停止使用，rinetd 这个工具，使用Nginx进行转发。

## 停用rinetd使用Nginx
### 修改Nginx配置
Nginx的配置文件，大概就这样写：

先在某路径下面创建好咱们的nginx的conf文件，里面写入这样的内容

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571656784005.png)

然后再 events {} 后面，把steam{} 写好，里面将咱们的conf包含进去就好，为了防止以后想添加更多的conf又要修改主配置文件，所以把include路径中所有的conf都load进来，用\*.conf来表示就好
如下

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571657157327.png)

OK，咱们到底需要开放什么端口呢？咱配置文件也不能瞎写啊，要转发什么端口？
我这这里列一下吧：分别是

25、465、587、995、993、101、143 这几个端口。
### 打开防火墙
要是像本人这样，用的阿里云服务器，记得还要去服务器里面把**防火墙**对应的端口开一下。

接下来就到激动人心的DNS解析配置时间了
对不起~我域名也是阿里云里面买的~，所以还是以阿里云为例演示
### 进行域名解析
登录控制台之后，找到**云解析DNS** ，找不到的同学，可以这样找

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571657615555.png)

打开之后，选择自己的域名进行解析设置

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571657714243.png)

然后添加几个记录、以本人的为样

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571658418224.png)

添加一个记录的过程，如下所示

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571657931632.png)

参考上面的图，对准主机记录 以及 记录类型，把那几个记录值都添加好之后。可以说完成了95%的工作了。

## 基本完成，打开MailPlus试试
然后就可以回到NAS里面，打开一下MailPlus

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571658057369.png)

进去之后，可以试试发个邮件，看看能否成功发出，然后，测试能否正常接收邮件

这里有个坑，一般发邮件之后，遭到退信情况的话，大都可能是**SPF**没配置好。这个SPF坑，大家遇到的话，去查一下该咋个配置就好，挺好处理的。这里不展开说~

给个测试过程出来，看看。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571661180135.png)

这是使用 自己搭建的服务发送的


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571661200375.png)

这个是我另外一个邮箱接到邮件后，回复一下~


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/NAS邮件服务器/1571660132121.png)

可以看到成功接收到了，打完收工

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>