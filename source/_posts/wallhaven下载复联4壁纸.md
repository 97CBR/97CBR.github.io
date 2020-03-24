---
title: Python自动下载Wallhaven复联4壁纸
tags:
  - Python
  - 爬虫
  - 工具
categories: 爬虫
keywords:
  - Python
  - 爬虫
description: 利用Python创建爬虫下载QQ音乐MP3文件
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556333485160.png
abbrlink: 153fb74d
date: 2019-04-27 00:00:00
---

## 看完电影想换壁纸了

复联4大家都去看了木有呀？这都上映好几天了，该被人家剧透的也差不多了吧。
所以，拒绝剧透，从我做起。都学学人家奇异博士吧，人家都看了1400万次结局都没剧透。

嗯，好看，很好看，票价很值
但是昨日好友碎碎念叫我给他写个下载寡姐壁纸的爬虫
还保证给我写这个爬虫不亏，这网站的壁纸质量太好了
所以，我就顺手写了一个爬虫去下载壁纸吧。
我的乔巴壁纸也好久木得换了。

![我的桌面壁纸](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556333224540.png)

不扯这么多，咱们准备撸起袖子加油干哈

## 写代码前的分析

### 分析一下网站

首先搜索一下复联4的标题，看到壁纸的预览图，似乎挺不错的。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556333485160.png)

OK ， 点进大图看看，看一下原图能有什么分辨率。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556333591608.png)

嗯，可以的，这分辨率挺不错的。
### 分析数据交互方式
那我们就准备分析数据请求方式啦
在这篇里面，我就不说太详细步骤了，我说个思路和过程哈
注意里面的一些看起来常规又不常规的技巧哈

在滑动页面去加载新的图片的时候，发现链接处是在不停变换的。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556333778781.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556333854038.png)

想必大家都发现了主要变换处就是page了

那么我们就可以猜测一下，数据的刷新是通过page改变来改变的

开启F12大法后，去观察网络加载项

滑动更新一页，可以看到，search这个请求比较突出

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556334016207.png)

我们看一下返回的数据

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556334070947.png)

看不出什么，我们就回页面，看一下第五页的图片链接

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556334116562.png)

右键检查，找到链接，猜测后面那个数字就是标识图片的id
回到请求返回页面，搜索 595400

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556334194872.png)

说明，我们要的数据，其实已经是包含在这个search 请求里面了。

OK，我们就可以不用管这里了。来到第二个高清原图页面。

右键检查图片元素，找到图片的链接。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556334298959.png)

OK，看的出他是在 img 标签， id为 wallpaper 的标签里面。

依旧刷新这个页面，看返回数据，搜索图片ID

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556334470996.png)

没错了，这个就是我们要的东西了

这个时候，已经可以说是把流程以及需要请求的页面给弄清楚了

## 开干写代码

我们打开pycharm，导入我们要的包。

一路写下去，写到请求原图链接的哪里，通过img 标签 wallpaper 的 id 居然没拿到数据

我们就单独请求一个页面，看数据到底数据在哪里

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556334784839.png)

看到数据在 meta 标签里面，property为"og:image 的标签里面
<meta property="og:image" content="//wallpapers.wallhaven.cc/wallpapers/full/wallhaven-532146.jpg" />

为什么我说数据就在这个里面呢？而不在其它的匹配标签里面呢？

细心看的朋友肯定发现了上面我找到原图链接的时候，链接中间会有/full/，其它的就不是

OK，稍作修改，数据就已经可以正常回来了
接着就是请求原图链接，并保存了。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556335077853.png)

代码放这里
``` python
# 我的代码呢？
```

完结撒花，关掉看下一篇吧

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556335178412.png)
***

你以为就这样就完了吗？还没看复联4的同学，看完后，请听完它的歌。因为事后有人说，电影的最最最后面，你会听到
叮~ 叮 ~ 叮~的打铁声
这个彩蛋有多少人被多少媒体误导没有彩蛋而错失哟

## 其实代码还可以优化
我们代码其实还没写完。上面只是常规的写法，和思路。并不是最优解。
我们回到分析过程，观察观察，你就会发现，其实我们并不用请求第二个原图页面也照样可以把原图给download下来的。

为何可以？

细心点看这几个链接

https://alpha.wallhaven.cc/wallpaper/637560
https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-637560.jpg

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556335499272.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556335571626.png)

我们怎么通过第一个直接构造出第二个链接？
将子域名alpha换成wallpapers，然后将637560换成full/wallhaven-637560.jpg就可以了啊

但是有些链接是6位数，也有些是5位数的。我们该怎么处理？直接用Python切片肯定不行的了。
还是细心，有些朋友可能以及发现我已经在上的那张图片哪里用来个箭头指向了一个叫data-wallpaper-id的属性

所以，我们不费吹灰之力就拿到要的数据了。
代码写起来

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-27/wallhaven下载复联4壁纸/1556336557313.png)



代码的话，我就放那个短一点的

``` python
import random
import time
import requests
from bs4 import BeautifulSoup


class haven:
    def __init__(self, name='marx'):
        # self.main_url='https://alpha.wallhaven.cc/search?q=id%3A1957&page={}'
        self.search = name
        self.main_url = 'https://alpha.wallhaven.cc/search?q={}&page={}'
        self.session = requests.session()
        self.headers = {
            "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36",
            "cookie": "__cfduid=d1bc48e470ea703ef259393d775e2f5361542527467; _pk_ses.1.1f04=1; _pk_id.1.1f04=bfd774c029e9a2e1.1542527473.11.1556277865.1556277657.; wallhaven_session=eyJpdiI6IlhHTzhnNmRFelhMdzBZWDdDdzZuUnVwdEh0M2pyMmN3dWo5dHpKS1orSGM9IiwidmFsdWUiOiJoeFFBb0FkWDN2OWpnTTYzaTRwcjgzekp1andjZmpCZDYrTkZYckVXeFFpK2hMSjh3RU5EMzJaM1RYRzR6RG8wVTNocXlDQ29UMDIrUlM1MUVXSjZlQT09IiwibWFjIjoiNTY1YjMyNzNlNTg3NjA0ZDIxZDAzYTI1MTY2NzQzOTRmNTdkMDg1MjZkMDkwZTVlZmE4Y2U3MTI1MjNjMjFkMSJ9"
        }
        self.jpg = 'https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-{}.jpg'
        self.png = 'https://wallpapers.wallhaven.cc/wallpapers/full/wallhaven-{}.png'

    def download_image(self, image_id):

        with open("./image/{}.png".format(image_id), 'wb') as f:
            statu = self.session.get(self.jpg.format(image_id))
            if statu.status_code == 200:
                f.write(statu.content)
            else:
                f.write(self.session.get(self.png.format(image_id)).content)

    def run(self):
        for page in range(1, 21):
            get_url = self.main_url.format(self.search, page)
            page_data = self.session.get(get_url, headers=self.headers)
            page_data.encoding = page_data.apparent_encoding
            soup = BeautifulSoup(page_data.text, 'lxml')
            temp = soup.find('section', class_='thumb-listing-page')
            all_li = temp.find_all('li')
            for temp in all_li:
                try:
                    image_id = temp.find('figure').get('data-wallpaper-id')
                    self.download_image(image_id)
                    time.sleep(random.randint(1, 4))
                except:
                    ...

haven("Avengers: Endgame").run()
```

## 完结撒花
诶，这不就可以完结撒花了嘛

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>
