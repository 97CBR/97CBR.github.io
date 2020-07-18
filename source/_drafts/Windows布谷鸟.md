---
title: Windows下部署使用Cuckoo布谷鸟沙箱
tags:
  - 沙盒
categories: 信息安全
keywords:
  - Windows
  - 沙盒
  - Cuckoo
  - 部署
description: 布谷鸟沙箱-搭建虚拟机环境~多图杀猫警告
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/57e88a2064651acd26dc5a79abd2542f.png
abbrlink: aaaf4be5
date: 2020-06-20 16:43:44
---

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/fbaad8681a6ebecf281f0aa379ceeb1b.png)


![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/fbffa8a3e880702c7fce537a6240e25d.png)

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/ab963f66c818db33d2b621a50d2dcad1.png)



## 前言

上一篇文章已经说了我为何要在Windows下部署Cuckoo，就是为了省点内存，提高体验。然后Cuckoo在Windows下在安装部署文档是空的。

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/d9e49e05ed1c15bea791a1efdaa184ad.png)

好一个 To be documented \~
幸好**Python的代码是跨平台的**，所以，不慌它，奥利给，干了
上一篇文章已经搭建好布谷鸟所需要的虚拟机环境（即沙箱），这里主要记录cuckoo搭建的一些小坑

## 安装Python27

因为Cuckoo的安装要求是在Python2下面的，所以安装Python27

[Python2.7.18 下载页面](https://www.python.org/downloads/release/python-2718/)

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/0ae942b2abf77eb0c011cb8dc8e98e83.png)

下载回来之后，安装好。

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/b7fa84b29c2d3fe96ce1e5c0b243dea8.png)

## 安装Cuckoo

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/2f2f23ad0ecc0058bc6c4fbc4710dca1.png)

U1S1,Pycharm他们Jetbrain家 的loading图是真的帅。

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/bf78cd85d39a6cba1ddcb9607ee0843a.png)

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/684f76e6209c2bd497ed9978fef8e906.png)

{% note info %}

在这个步骤如果出现GMS、Crypto库编译错误什么的，可以安装VC的一个Python环境即可
下载安装 [Microsoft Visual C++ Compiler for Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266)

{% endnote %}

## 安装PIL、Pillow

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/3c2b1d051e59187746715fcf117f3c2e.png)

## 安装pymongo

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/36d8c65f71790c95e2d6c66c1fcc7296.png)

## 初始化Cuckoo

> cuckoo init

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/878926c13f84f19ae46b3c66afd0b39d.png)

{% note info %}
如果出现启动错误，那就需要将用户目录下的.cuckoo目录删除掉
{% endnote %}

## 启动Cuckoo

> cuckoo -d

安装了VirtualBox 6.1 ，但是还是出现了这个报错
Potentially vulnerable virtualbox version installed. Failed to retrieve its version. Update if version is: 6.0.5

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/11a895a82eb1869dcfabe755093a5a3b.png)

**修改配置文件，跳过检测**

``` xml
<!-- C:\Users\你的用户名\.cuckoo\conf\cuckoo.conf -->
ignore_vulnerabilities = yes
```

>修改好之后，启动看看，可以发现找不到vboxmanage
>报错提示我们，我们还有配置文件没修改，找到virtualbox.conf，修改path的值为我电脑上的virtualbox的目录下的vboxmanage路径

2020-06-21 10:55:00,121 [cuckoo] CRITICAL: CuckooCriticalError: VirtualBox' VBoxManage not found at specified path "/usr/bin/VBoxManage" (as specified in virtualbox.conf). Did you properly install VirtualBox and configure Cuckoo to use it?

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/d3989ef69686a011e9645c6d0fdfeb31.png)

```xml
<!-- C:\Users\你的用户名\.cuckoo\conf\virtualbox.conf -->

# Path to the local installation of the VBoxManage utility.
# path = /usr/bin/VBoxManage
path = E:/Program Files/Oracle/VirtualBox/VBoxManage.exe
```

>修改好之后，再启动看看，报错提示我们没有cuckoo1这个虚拟机，找到virtualbox.conf，修改path的值为我电脑上的virtualbox的目录下的vboxmanage路径

2020-06-21 11:04:12,516 [cuckoo] CRITICAL: CuckooCriticalError: Please update your configuration. Unable to shut 'cuckoo1' down or find the machine in its proper state: The virtual machine 'cuckoo1' doesn't exist! Please create one or more Cuckoo analysis VMs and properly fill out the Cuckoo configuration!

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/14ab5166f5f1f8ae0445b0d055ed5394.png)

```xml
<!-- C:\Users\你的用户名\.cuckoo\conf\virtualbox.conf -->
[cuckoo1]
# Specify the label name of the current machine as specified in your
# VirtualBox configuration.
label = XP_64
```

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/e0644e7467bd45a247a46415b9d91721.png)

>修改好之后，再启动看看，报错提示还原虚拟机的快照失败，叫我给虚拟机搞个快照再说。所以，就给他整个快照噻\~

2020-06-21 11:11:57,365 [cuckoo.machinery.virtualbox] DEBUG: Stopping vm XP_64
2020-06-21 11:11:57,552 [cuckoo.machinery.virtualbox] DEBUG: Restoring virtual machine XP_64 to its current snapshot
2020-06-21 11:11:57,634 [cuckoo] CRITICAL: CuckooCriticalError: Error initializing machines: VBoxManage failed trying to restore the snapshot of machine 'XP_64' (this most likely means there is no snapshot, please refer to our documentation for more information on how to setup a snapshot for your VM): error code 1:

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/5b4d505985738c1d1e74c91e3c3bceb1.png)

所以，给它整了一个快照

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/4167243b791750b6243c9c7f6dcae333.png)

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/b66abbca83b982af8122aaf056796326.png)

>拍好快照之后，再启动看看，看着这个Waiting for analysis tasks. 老夫露出欣慰的笑容 (ಥ_ಥ)

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/08fc798b4650cb77cec561ba11a55d0f.png)

## 启动Cuckoo Web界面

> **cuckoo web**
> 可以看到提示我们，需要配置MongoDB

In order to use the Cuckoo Web Interface it is required to have MongoDB up-and-running and enabled in Cuckoo. Please refer to our official documentation as well as the $CWD/conf/reporting.conf file.

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/9bfac5c410419cc23a04fbf7634e5cdb.png)

### 下载MongoDB

先跑去下载一个MongoDB
[MongoDB下载地址](https://www.mongodb.com/try/download/community)

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/d07fb1af5c1a36156a621115ee6e4f51.png)

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/6d5c56358664b718b6e725343367c1fd.png)

安装好了就好了

### 修改MongoDB的配置文件

> 找到配置MongoDB的文件，是reporting.conf，打开它，修改一下

```xml
<!-- C:\Users\你的用户名\.cuckoo\conf\reporting.conf -->
[mongodb]
enabled = yes
host = 127.0.0.1
port = 27017
```

### 创建cuckoo链接

改好了配置之后，MongoDB也在本地测试能使用之后
>cuckoo web 启动一波，给了我一个新的错误

$ cuckoo web
C:\Python27\python.exe: can't open file 'C:\Python27\Scripts\cuckoo': [Errno 2] No such file or directory

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/fce4e0320bc29ba65f0a65f7604f93a8.png)

为此，我用mklink将cuckoo链接过去，然后再跑一遍，就给了我一个这样子报错

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/bb5ad3b315799805089eda93cc402bf9.png)

没办法，只能去翻翻资料，发现挺多人都遇到这个问题的，GitHub上面挺多关于这个报错的issue，幸好都被close了

测试了两种方案，一钟mklink cuckoo.exe 一种mklink cuckoo-script.py

- mklink cuckoo.exe

  ![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/d7ae15ba196b1c1927ea5943caf22100.png)

- **mklink cuckoo-script.py**
  ![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/b73648c20da39c999a07112c9ca30a6f.png)

可以看到mklink py文件的那个方案是正解

## 访问Cuckoo网站

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/880584a4f0b413f7872f9de853525a2e.png)

喜大普奔(ಥ_ಥ)
其实是可以看到是有点儿不正常的，这个先不管，后面再处理

## 配置网卡（大坑）

最开始，没了留意到装好了的XP系统居然没有网卡

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/01a9a052fa1b4e5aaf708d1c212cf08b.png)

设备管理器里面也显示网卡驱动有问题，一度导致我想下载一个万能驱动包给他装上
但是，太麻烦了那样子。毕竟VBox要是不支持XP的网卡的话，也太捞了吧。所以，自信认为问题出在VBOX身上。
接着去搞配置，看着这灰色的网卡选项，我一度很绝望。心想：改不了的嘛，什么情况，太辣鸡了吧。

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/f37b13fa3f231e5e6dbd57ffaeb55918.png)

然而发现是自己蠢了，虚拟机都没关，就挂起在那里，这让人家怎么改嘛。。。
所以，关掉虚拟机之后，就重新配置一下网卡

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/6f0a865de6fc17168181b4ad4838f865.png)

> 选一个适合自己系统的，要是不知道哪个是系统支持的，可以像我这样，多开几个网卡，选多几个，到时应该总有一个能用的

从新打开网卡设置页面看看

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/91705c7311a248f6257b7deba1f0de8e.png)

再ping一手，奈斯

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/6b2b6cd22e3941ad6d4396d65e8e2171.png)

## 根据IP地址修改一下IP相关配置

虚拟机网卡的问题修好之后，该配置cuckoo的配置文件了。稍微根据自己的虚拟机实际情况处理一下即可

```xml
<!-- C:\Users\你的用户名\.cuckoo\conf\virtulbox.conf -->
# Specify the IP address of the current virtual machine. Make sure that the
# IP address is valid and that the host machine is able to reach it. If not,
# the analysis will fail.
ip = 192.168.1.232
```

``` xml
<!-- C:\Users\你的用户名\.cuckoo\conf\cuckoo.conf -->
[resultserver]
# The Result Server is used to receive in real time the behavioral logs
# produced by the analyzer.
# Specify the IP address of the host. The analysis machines should be able
# to contact the host through such address, so make sure it's valid.
# NOTE: if you set resultserver IP to 0.0.0.0 you have to set the option
# `resultserver_ip` for all your virtual machines in machinery configuration.
ip = 192.168.1.166
```

## 配置客户机（沙箱）

安装增强工具

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/480adf9ad07acf389213a268f7b8d51f.png)

重启一下虚拟机

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/d4bdfc801d2d2befd0615abee9153beb.png)

下载一个符合虚拟机版本的Python2安装包，扔进去，我这里是32位的Python27。

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/29a3bbe3879033ddee8e8accc27498e5.png)


然后，去**C:\Users\你的名字\.cuckoo\agent**这个地方拉agent.py文件进去，双击运行

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/f28e79739d5224f7dc214d040c65cc8b.png)

> **建议在此处给它来个快照**

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/82ef8c154bfd9774c7d70f2c15f7f571.png)


如果出现拖曳文件进虚拟失败的问题，可以采用共享文件夹的方案


![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/cf15ac245bb6a021f32b2ef9f69f9589.png)


![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/b65fc62573b67e4bfb903bd283a17198.png)

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/84d9ece37947617f1f30b915599bdfb3.png)


## 尝试修复cuckoo web 的异常

cuckoo -d
cuckoo web

启动一下后台服务以及web服务

- 注意，没有我下图截的那个红色框的 web 启动界面都是不正常的启动。

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/32ef1bbb2942970f4c0ca34e8bc67404.png)

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/6117af4b036f65c98dfc112a8cacd47d.png)

正常和不正常的区别咋一看不大，就差了一个上传的图标。实际上，不正常的话，是无法上传病毒样本的。
解决方案也比较玄学，这里随意说说。当时测试，发现有几个js文件会load失败，事关socket传输的问题。一系列尝试无果，然后便扔到服务器OSS中，在线load它。然而，还是测试不正常，一气之下，打尻去了。第二天过来重新启动便可以正常启动了。太神奇了\~\~\~

## 配置tcpdump


![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/2c53a5839321e88597b36f80d1ef97a0.png)



[点击下载 Microolap TCPDUMP for Windows](http://www.microolap.com/products/network/tcpdump/download/)
然后解压好，放tcpdump到.cuckoo目录下
修改配置文件

```xml
[sniffer]
# Enable or disable the use of an external sniffer (tcpdump) [yes/no].
enabled = yes

# Specify the path to your local installation of tcpdump. Make sure this
# path is correct.
# tcpdump = /usr/sbin/tcpdump
tcpdump = C:/Users/Marx/.cuckoo/tcpdump.exe
```
![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/dbe6c1f710cd990aadfa60405f9bef39.png)

- 不过在后期测试的时候，似乎不太正常，所以这个后期再处理下


## 上传病毒样本

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/e295335e4efc4610e3eb59c62f204d3f.png)


选中了待分析的软件之后，分析它

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/4a5ee0e55599a77de2f74cf61d811618.png)


在cuckoo -d 的日志下，可以看到分析进展，是否正在正常运行分析

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/7634dc64960f9ce8620cdc57bcdd06b1.png)

在沙盒里面可以看到病毒运行的亚子

![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/4fb7eaa2a4f392b5c3b69acdf2358bb3.png)



![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/5f15c8c3418b8869b0813378833d22e1.png)


![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/f452b3442cfd95fd9aeeb36e708a5956.png)

点击就可以看到报告了

## 查看报告

这是一个比较正常的软件的一个分析

- 概要报告
![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/53fd1516887e1ac75337faa27fc77786.png)

- 静态分析
![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/acf2291b345a38cf043bcb6a646219ee.png)

下面的是一个病毒的分析报告

- 概要报告
![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/c65cfbfcf9b09a95aedee416fed59d7e.png)

- 行为分析
![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/b4470d008997a255b567204477a68583.png)

- 释放文件
![Windows布谷鸟](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/HEXO/Windows%E5%B8%83%E8%B0%B7%E9%B8%9F/ac23335cae8d838eccc672cbdaa3b07c.png)

## 结语

差不多啦，布谷鸟的部署，花了我两三个下午。虽说整体部署不是很复杂，遇到错误，fix掉配置的问题就可以了。然后总有玄学错误，让人无法排除。说明Cuckoo的稳定性和健壮性是有待提升的。但是问题不大，毕竟是能解决并正常使用的。就是怎么提高用户部署和使用体验的问题了。接着，Cuckoo的官网是没哟Windows下部署的说明文档的，希望能给看到的同学一些帮助，OK\~ Easy\~



