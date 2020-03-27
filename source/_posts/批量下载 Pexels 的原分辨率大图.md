---
title: 批量下载 Pexels 的原分辨率大图
tags:
  - 爬虫
  - Python
categories: 爬虫
keywords:
  - 爬虫
  - Python
  - 批量下载
  - 原分辨率大图
description: 批量下载 Pexels 的原分辨率大图
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555244009850.png
abbrlink: a267f10a
date: 2019-04-14 00:00:00
---

## 担心侵权，狗命要紧

前段时间的开源中国成功地告诉了我们要注意版权保护意识，诶~就，人家有很多图片upload到网上，咱们也是不能随便用的，万一，我说的是万一，一不小心就侵权了怎么办？

惹不起惹不起~

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexel_的原分辨率大图/1555239516357.png)

所以，我文章用到的题图就不能随意找了，对吧。
刚好前面那些文章基本把我珍藏已久的图片给用完了，又不想重复用怎么办嘛？

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexel_的原分辨率大图/1555239677093.png)

## 问问度娘

赶紧问一波度娘：度娘度娘，我该怎么找那些无版权的图片呀。
度娘：你可以到Pexel这个网站里面找找哈。

其实，度娘是这样告诉我的…………

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexel_的原分辨率大图/1555239901856.png)

其实，我之前有一篇说过怎么去广告的，大家出门左拐看看那篇文章就知道怎么操作了。

## 打开Pexels瞅一眼

OK，那我们就去Pexels这个网站看看效果怎么样吧。

网站链接在这里 [Pexels](https://www.pexels.com/)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555240143588.png)

挺舒服的，点击图片，会悬浮出一个窗口，可以选择分辨率下载。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555240199008.png)

点击原图下载，OK。扑街了。。。要验证码，下面有个提示，说：登录网站后，不用验证即可下载。（PS：当时忘记截验证的图了，大家知道有就好哈）
那还在考虑什么哟，注册一个！
一顿操作后，就注册好了。点击下载，果然就再也不提示要验证了。十分舒服呀。

然后……就沉迷在这个网页里面去疯狂找图片，然后下载。。。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555240747058.png)

作为一名21世纪的新青年，这样下去不是办法呀。照这个效率找，今天就没这篇博文了

## 所以写个爬虫自动帮我下载

OK，那就动手，写个爬虫，批量下载这个Pexels的原分辨率图像和1920分辨率的图片。
### 准备思路
我们写代码、写bug前要想好思路再写，要不就是浪费生命啊。

>1、F12大法启动
>2、分析网页数据加载方式
>3、分析请求规律
>4、厘清细节
>5、动手写代码

- 第一步：我们先把F12大法启动，切换到network选项。照常把数据清空了，把缓存禁用。
- 第二步：将鼠标往下滚动，看到network里面有变化就可以了
- 第三步：数据太多，但是可以看到很多都是JPEG的加载，我们看到页面并未发生跳转，很容易看出是异步请求加载数据回来的。切换选项卡到 XHR 选项。
容易看到请求链接长这个样子

>https://www.pexels.com/?format=js&seed=2019-04-14%2B10%3A59%3A08%2B%2B0000&dark=true&page=3&type=
>(解码后：)https://www.pexels.com/?format=js&seed=2019-04-14+10:59:08++0000&dark=true&page=3&type=

### 分析返回数据
看一下返回的数据

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555241631978.png)

居然是js代码……很强，但是我不慌~嘿嘿嘿

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555241688781.png)

### 美化分析一下下

先将这一坨坨的代码美化一下下。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555241903395.png)

仔细观察一下数据。就发现了很多photo和https的字样。
那，十有八九是将图片的数据返回后，js将内容插进去的。
所以，我们先放一下，看一下图片的链接长什么样？下载图片的链接又长什么样。

点击下载两张图片，复制好下载链接到 Sublime 里面去，观察一下。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555242219579.png)

### 找到规律

发现关键的地方就那个**数字**，还有**dl对应的参数**，后面的w和h就是图像的**宽高**了。

我们观察好下载链接的区别后，就look一下刚才的js文件里面有没这种规律。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555242828340.png)

emmm，符合我们的预期。那几个关键参数都有了，除了那个图像分辨率的参数没有。

用Python请求刚才的链接回来，然后，再用正则匹配一下所有的链接。就匹配好这个http就好了。

通过数据请求回来，看到了很多无关的链接。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555243146074.png)

这个时候，我们再过滤掉我要的URL就好了。

这个时候，我们已经可以下载图像了。但是，很明显，这样下载下载的图片分辨率不是我们要的。
说好的高清大图，原分辨率大图呢?
所以，我就必须找到分辨率参数在哪里，要拿到它。因为每个图片的分辨率都是变化的，没有统一的分辨率，除了1920x1080这个是通用的。原图的都是随相机变化而变化的。

### 再分析一下，解决原图分辨率问题

我们回到相片详细页面，检查元素看看卡，原图分辨率在那个位置。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555243262376.png)

再去请求一下页面，看返回数据，有没包含这个分辨率。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555243328703.png)

分辨率是直接返回回来的，那就好办了。用BeautifulSoup解析一下就好了。

这个时候，我们还没有拿到 类似这样的链接 https://www.pexels.com/photo/blurred-background-bokeh-bubbles-close-up-2068411/
怎么办？这个时候，经验直觉告诉我，我把链接改成https://www.pexels.com/photo/2068411/
他也是会照样给我返回到这个图片的详细页面的。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555243594874.png)

测试了一下，的确是返回到这个页面的。美滋滋呀美猪猪。

## 分析结束，确定流程以及参数

这个时候，我已经拿到我们要爬的所有需要的参数信息以及流程了。
>1、从请求页面入手，拿到js里面包含的http链接
>2、过滤链接后，截取URL，去请求图片详细页面
>3、用BeautifulSoup解析拿到原图分辨率
>4、下载

由于我们前面有提到注册后，不会让你点击验证码，所以，这次爬虫，我们要用到cookie来做。

## 简单介绍Cookie

Cookie是什么？
>Cookie总是保存在客户端中，按在客户端中的存储位置，可分为内存Cookie和硬盘Cookie。可分为非持久Cookie和持久Cookie。
>因为HTTP协议是无状态的，即服务器不知道用户上一次做了什么，这严重阻碍了交互式Web应用程序的实现。在典型的网上购物场景中，用户浏览了几个页面，买了一盒饼干和两瓶饮料。最后结帐时，由于HTTP的无状态性，不通过额外的手段，服务器并不知道用户到底买了什么，所以Cookie就是用来绕开HTTP的无状态性的“额外手段”之一。服务器可以设置或读取Cookies中包含信息，借此维护用户跟服务器会话中的状态。


## 先给大家看看效果
下面给大家看一下效果：

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-14/批量下载_Pexels_的原分辨率大图/1555244009850.png)

这个文件夹里面有下载的所有图片，每张图片有两个分辨率，1920x1080的，原分辨率的。通过名字_xxxx这个xxxx来快速标识分辨率为多少。

代码有两份，一份是单线程采集、下载的。
另一份是多进程采集下载的，单个进程采集一个页面后的所有图片后，再多个进程去下载，可以有效提高下载效率。

## 代码呈上

在这里，我贴出单进程的那份代码吧。供有兴趣的同学现场观看。

``` python
# -*- coding: utf-8 -*-
# @Time    : 4/14/2019 12:43
# @Author  : MARX·CBR
# @File    : __init__.py.py

import requests
from bs4 import BeautifulSoup
import re
import time
import lxml


class DownloadPexelSpider:
    def __init__(self):
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36',
            'cookie': '换成自己的：__cfduid=d6abd7c099adcbbee835df61de3e6ef3c1555167983; locale=en-US; ',
        }
        self.download_image_url = []
        self.raw_url = []
        self.session = requests.session()

    def change_page(self, page):
        ou = 'https://www.pexels.com/?format=js&seed=2019-04-14%2004%3A22%3A06%20%2B0000&dark=true&page={}&type='.format(
            page)
        r = self.session.get(
            ou,
            headers=self.headers)
        print(ou)
        r.encoding = r.apparent_encoding
        data = r.text
        pattern = re.compile(r'http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+')  # 匹配模式
        url = re.findall(pattern, data)
        # print(url)
        for i in url:
            if 'dl' in i:
                if i not in self.raw_url:
                    # print(i)
                    self.raw_url.append(i)
        print(self.raw_url)
        for g in self.raw_url:
            try:
                self.get_download(g)
            except:
                print('error', g)

    def get_download(self, url):
        # tt='https://images.pexels.com/photos/2116222/pexels-photo-2116222.jpeg?cs=srgb&amp;dl=chair-furniture-indoors-2116222.jpg&amp;fm=jpg'
        tt = url
        newtt = tt[:41:].replace("images", 'www').replace('photos', 'photo')
        print(newtt)
        r = self.session.get(newtt, headers=self.headers)
        # print(r.text)
        soup = BeautifulSoup(r.text, 'lxml')
        for k in soup.findAll('input'):
            if k.get('name') == 'download-size':
                size = k.get('value')
                size = size.split('x')
                if int(size[0]) > 1920:
                    structure = "?dl&fit=crop&crop=entropy&w={}&h={}".format(size[0], size[1])
                    print('查找成功')
                    self.download_image_url.append(tt + structure)
                    self.single_file_download(tt + structure)
                    break

    def single_file_download(self, url):
        i = url.replace('\\', '').replace('\'', '')
        print(i)
        name = i[33:39:]
        w = i[-11:-7:]
        print("准备下载", i)
        with open('./images/{}_{}.png'.format(name, w), 'wb') as f:
            raw = self.session.get(i, headers=self.headers)
            content = raw.content
            f.write(content)
        i = i[:-11:] + "1920&h=1080"
        with open('./images/{}_1920.png'.format(name, w), 'wb') as f:
            raw = self.session.get(i, headers=self.headers)
            content = raw.content
            f.write(content)

        print("{}下载完毕".format(name))

    def run(self, s, e):
        for i in range(s, e + 1):
            print(self.change_page(i))


if __name__ == '__main__':
    DownloadPexelSpider().run(10, 20)
```

代码该怎么写，上面已经将思路都给过一遍了。具体写起来就很快了，语法都是基本语法，也没有什么坑。有兴趣的同学可以试着自己去写一遍，然后再看一下我怎么写的。

----------

本篇文章大概就到这里了。一篇文章下来，我后台已经下载了500+张好看的图片了。大家可以到我GitHub里面fork代码下来看看，快速下载大量好看的图片，你不心动吗？
代码已经同步上传到GitHub，包括一个单进程的、一个多进程的代码。
项目地址在这里：[PexelsSpider](https://github.com/97CBR/PexelsSpider)

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>