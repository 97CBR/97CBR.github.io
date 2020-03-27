---
title: VMware Workstation 安装以及Linux虚拟机安装 指北（保姆级教程）
tags:
  - 虚拟机
  - Linux
  - VMware Workstation
categories: 爬虫
keywords:
  - VMware Workstation
  - 虚拟机安装
  - Linux
description: VMware Workstation 安装以及Linux虚拟机安装 指北
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553932753054.png
abbrlink: e679d64e
date: 2019-03-30 00:00:00
---

## 虚拟机是个啥

最近有挺多小伙伴跟我说起虚拟机这个东西，所以，今天就给大家写一篇虚拟机安装使用指北吧。
虚拟机（英语：virtual machine），在计算机科学中的体系结构里，是指一种特殊的软件，可以在计算机平台和终端用户之间创建一种环境，而终端用户则是基于这个软件所创建的环境来操作软件。（该段说明来自wiki）
我们即将安装的软件 VMware 则是系统虚拟机。可以轻松在一个操作系统上面安装多一个或者多个操作系统，如kali、Ubuntu、centos等Linux，windows系统，甚至Mac系统都可以。

## 安装VMware
那OK，下面就进入VMware安装部分
首先，你得有VMware的安装软件，可以选择到官网下载，也可以到百度、谷某 引擎上面搜索VMware即可。大家注意别下载到那些看起来就很捞的软件即可（很多下载站里面的软件都很假的，或者带毒的，最好还是去官网下）。然后，有资金充足点的可以选择支持正版，最好就支持正版，别人开发出这么流批的软件也是很厉害啦。手头没这么宽裕的就自己搜索对应下载版本软件的激活码就好了……
附送 [下载链接](https://my.vmware.com/en/web/vmware/info/slug/desktop_end_user_computing/vmware_workstation_pro/15_0)

### 打开VMware安装包
OK ,软件已经下载好了，我就不下一次了，之前下载安装过了。漫长的下载后，接着就打开软件，VMware-workstation-14.exe
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553925308463.png)
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553926019253.png)

一路Next下去，他会提示修改安装位置，在这里推荐修改一个位置比较好，可以安装在一个你自己喜欢的位置，默认安装在C盘，还是太伤我C盘的空间了。(C盘一个T空间的请忽略……)
在这里我安装到E盘，因为我E盘里面安装的都是编程IDE和开发环境。所以，大家根据自己的需求来装就好。
下面的那个选择框是说安装一个增强型键盘驱动程序，自己看情况选择。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553926081656.png)
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553926149488.png)

一路Next，最后Install就可以完成安装了。再点击Finish就好了。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553926235692.png)

启动后，界面长这个样子。然后点击Help，再选择输入授权秘钥就OK了。在这里再提醒一次，尽量支持正版哈。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553926380199.png)

到这里VMware的安装就完成了。接下来就是安装虚拟机了。

## 安装kali虚拟机

本教程以Kali Linux为例子，为大家安装一个 Kali Linux 操作系统。Ubuntu和centos和Windows系统的操作类似。Mac的稍有不同，需要改几个配置文件。有兴趣的可以自行查阅资料，我现在就不细说这个问题，后期可能会有一篇说明Mac虚拟机安装的文章吧~

OK，希望可以在VMware里面安装一个操作系统很简单，有两条，不，三条途径，或者更多……
最常规的就是下载该操作系统的官方镜像，然后装载进去安装即可。
也可以让别人安装后，将安装虚拟机的文件夹给拖过来，不是VMware的文件夹，是虚拟机的文件夹。
基于第二种，第三种就是去网上下载别人打包好的虚拟机文件夹的压缩包。。。（怎么感觉第三种有点牵强）

OK，咱们就挑最难的那种讲讲

### 第一种：镜像安装
下载kali的镜像，到官网下载页面 >https://www.kali.org/downloads/
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553927247277.png)

我们选择第三个，Kali Linux 64位的下载，中规中矩。用种子下载听说可以快一点哦。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553927465210.png)

几分钟过后，镜像下载好了。我们便来到VMware，准备安装系统。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553927822476.png)

接着进入引导页面，选择自定义高级安装

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553927957164.png)

然后一路Next

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553927998094.png)

然后，选择Linux，下面的版本选择debian 7.x 64-bit

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553928189136.png)

选择安装位置，在起一个狂拽酷炫的名字就可以下一步了。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553928351486.png)

接着选一下你希望你的虚拟机是几核几线程的选项

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553928422395.png)

下一步便来到了选择虚拟机内存页面，建议内存都很低……所以我还是建议按照自己的PC来选择多大内存吧。太小的话，你用起来开，太大的话可能会出一些不可预料的错误。。。我在这里选择4G的内存分给他。因为我有12G内存，给他1/3,不过分不过分。。。
网络类型，有四个，桥接、NAT（ 网络地址转换）、主机模式、不联网。我这里默认用NAT就OK。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553928769211.png)

下面的硬盘和控制模式就默认就可以了。。。一路next下去。
出现要你选择磁盘大小的时候，根据自己电脑情况自行判断选择。基于我1T的外置硬盘，我就分了50G空间给他。然后选择虚拟磁盘拆分就OK。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553928931855.png)

接着到虚拟磁盘存储位置选择的时候，就选择之前的那个文件夹就好了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553929078976.png)

下一步点完成后，会来到首页，点击编辑虚拟机设置

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553929115261.png)

来到设置页面后，选择CD/DVD项。再浏览刚才下载回来的iso镜像（PS：你想装什么系统就在这个时候选什么镜像）

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553929187227.png)

OK保存后，便可以开机了。Power on 启动起来！！！
用键盘的↑、↓选择岛图形化安装选项，回车。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553929323243.png)

然后选择安装语言，在这里建议大家选默认的English，多用英文可以有效提高英语水平哟，也可以避免踩一些中文的坑~~
回车后，叫你选区域，那就选Hong Kong咯。。。
然后就是选择键盘布局，有中国当然选中国呀。
回车后……便是一顿自动操作…………
终于停下来时i，叫你写hostname，用来在网络中分辨你的系统………以及下面那个域名，填自己喜欢的就好啦。
关键的是**设置密码**啦，后面都是要用的，大家设置后**千万要记住**哟，怕写错的，可以勾下面的显示密码来确认一下下

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553929806510.png)

接着一路continue，在分区的时候，要将NO改成YES。如下即可

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553929962229.png)

然后就不用操作了，一个钟半个钟后便安装完毕了。。。（安装在SSD的同学请忽略……）
后面的包管理选择、引导之类的（**注意GRUB是要安装到/dev/sda这个设备上别顺手就点过去了哦……会扑街的**），其他的相信大家都可以看懂然后默认继续就好了。
这里还是再提醒多一下吧

{% note info %}
注意GRUB是要安装到/dev/sda这个设备上别顺手就点过去了哦
{% endnote %}

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553930635743.png)

当你看到这个页面的时候，你已经成功98%了。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553930814977.png)

重启虚拟机系统后你就可以看到这帅的爆的kali Linux啦

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-30/VMware_Workstation_安装以及Linux虚拟机安装_指北/1553932753054.png)

### 第二第三种：文件安装
第二第三种很像，我就并在一起讲了。关键是拿到那个打包好的系统文件夹。
OK ，这里我有一个从别人主机上面拖过来的系统，解压后就进入VMware的首页


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-28/BogImages/1585330389377.png)

然后选择刚才解压出来的文件夹。这里我选择Ubuntu9的这个文件夹。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-28/BogImages/1585330427726.png)

点击Open便会自动加载进左边的主机列表啦。
然后开机，你就可以看到系统了。（很多同学都没见过Ubuntu9的样子吧，哈哈哈~~~）

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-28/BogImages/1585330435651.png)


## 结语

本篇不完全指北的关键不在于系统安装的步骤。因为每个系统安装的步骤都是一个样。用物理机来说，把光盘或者刻录好镜像的U盘怼进去后，开机按Esc或者F9之类的，选择启动的方式就可以进入安装系统的引导界面了。按照里面的安装提示选择自己的选项就好了。
在这里吐槽一句，WIN10的安装，如果它安装在MBR磁盘上，你不格式化磁盘转成GPT格式的话，会给你甩一句“无法在驱动器0分区上安装windows”的问题，这个问题，改天写一篇短文简单说明一下该怎么解决吧。

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>