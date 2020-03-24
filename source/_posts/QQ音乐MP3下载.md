---
title: QQ音乐MP3下载
tags:
   - Python
   - 爬虫
   - 工具
abbrlink: 3aaed7d
date: 2019-3-29 00:00:00
categories: 爬虫
keywords:
    - Python
    - 爬虫
description: 利用Python创建爬虫下载QQ音乐MP3文件
cover: https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-24/2020-3-24未命名文件/1585031420351.png
---


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553837004446.png)

<center>没错本次写的内容的对象是我们熟知的QQ Music。</center><br/>

本篇文章涉及内容包括：Python，爬虫，json解析，request 库的使用<br/>

## 缘起

前几天刷B站无意中又刷到了一首神曲，“I Want My Tears Back”，挺好听的。听了几遍后便寻思着能不能把这歌给下到手机上拿来当闹钟的，听过的同学应该知道这歌有多提神，哈哈哈~~~
没听过的同学可以点击文章上方的音乐，感受一下下。

## 动手分析

接下来，当然要选择一下从哪个平台下搞这首歌回来啦。网易云音乐和QQ音乐，选择哪个？那就从网易云入手吧，毕竟用的多些，接着便去网易云一顿操作，此处省略1000字描述。发现，哎呦，这网易云……不好搞呀。所以先不管了，看一下QQ音乐的情况先。
F12大法一开，QQ音乐就先给我来了个惊喜，大大符号图标倾情相送。对比云村的就没有啦。
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553838437967.png)
撇开这些不关键的东西不说了，接下来就是搜索一首歌。

这个时候，先把控制台切到network栏，这个时候，你会发现左下角有很多请求链接。请求各种各样的内容，这个时候，这些东西对我们都是没用的，是吧。我们要的是我们请求搜索那一瞬间他发出去的数据。所以，为了避免无关数据的干扰，建议点击左上角的清空把记录清空先。然后右上角勾上disable cache，避免缓存使我们看不到我们要的数据。
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553838797742.png)
ok，这个时候我们输入我们要查的歌，I Want My Tears Back。我们已经可以看到有东西已经发出去并返回回来了。就是这个链接
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553839050029.png)
> https://c.y.qq.com/splcloud/fcgi-bin/smartbox_new.fcg?is_xml=0&key=I%20Want%20My%20Tears%20Back&g_tk=1419336673&loginUin=824368xxx&hostUin=0&format=json&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq.json&needNewCode=0
>
我们观察一下，这里面的参数，key就有I Want My Tears Back这个歌名了，再看一下返回数据。是一个json数据，有很多内，我们可以很容易看出了数据包含了 专辑、mv、歌手、歌曲 这四大区域。我们关注歌曲这个栏，在count 中已经表明找到了两首符合度最高的歌曲。一首是Nightwish的一首是Hok-key的。因为我想要的就是 夜愿 这个版本的，所以我们就默认第一首出现的就是我们要下的歌曲哈。
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553839289035.png)
这个时候，拿到这些数据有什么用呢？歌曲链接还没不知道怎么构造呀。。。
先别急，我们看看歌曲链接长啥样。然后…………
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553839647380.png)
好尴尬呀，有木有，这，这谁忍得住呀？所以，机智的我打开了另外一首歌……
瞬间右边记录出来了一大堆，没错是一大堆东西……但是，控制台有分类呀，不怂。选择media分类，就给我们过滤出了那几个链接。
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553840243529.png)
<br/>没办法了，逐个点击。1分钟过后，I got it.最后那个链接就是歌曲链接。
> https://isure.stream.qqmusic.qq.com/C400000JHuWh4fOBxD.m4a?guid=8665097290&vkey=A78C373DCB421D9DA3B1C3A87979A32A6062159F032C2D44C83BDD8D9D1F717B429F6C92A6C2720A1DB18AC835D4E6FCD409D6F0D0CE6F21&uin=7642&fromtag=66

可以清楚看到有几个关键参数。guid、vkey、uin、fromtag，我们先暂且不管那个参数可以省略好吧。先看看可以从哪里找到这几个参数……又得看链接的返回值了（此处说明，可以在控制台点击response标签查看返回值，你只要按↓键就OK了）。在漫长的遍历过程中，我注意到了一个链接的内容十分不一样的，不相同的，很多很多。直觉告诉我，就是这个链接了。
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553840766388.png)
<br />拿去做一下URL解码后，咋一看一头雾水。不信你看一下下？<br />

接合歌曲链接，这个请求链接，搜索请求结果，这三个内容。其实不难发现，songmid 这个参数是关键。
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553841536865.png)
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553840915649.png)


<br/>所以，我们这个时候基本就以及厘清下载音乐的步骤以及思路了。首先，请求搜索某一首歌，然后或者到song mid 接着song mid 去请求服务器拿到这首歌的播放链接。接着用request请求数据回来再用二进制保存就OK啦。

## 代码写起来

导入需要用到的json库和requests库。

构造一个类，downloadMusic，初始化一个headers，做个最简单的反爬。
写run方法，将上面的思路实现出来。请求回数据后就用json.loads方法加载json数据。逐步逐步请求服务器，填充需要填的数据。
最后拿到链接的URL后，就easy了，用requests请求资源回来后。用.content,不要用.text。又人问过我这两个方法的区别。
简单来说，content拿到的数据是字节，调试打印出来你会发现数据前面会有个b'，text的话，就是一个字符串了。
因为我们要保存歌曲，就肯定要用字节保存好，用wb方式打开，然后写进去后便关闭即可。代码贴在下面。
至此写完收工，可以美滋滋地下载我们要的歌了。有了它还怕什么铃声找不到自己喜欢的问题吗？<br/>

``` python
# -*- coding: utf-8 -*-
# @Time    : 10/10/2018 9:31 PM
# @Author  : MARX·CBR
# @File    : __init__.py

import requests
import json

class downloadMusic:
    def __init__(self):
        self.headers = {
            'Accept': 'text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8',
            'Accept-Encoding': 'gzip, deflate',
            'Accept-Language': 'zh-CN,zh;q=0.8,en-US;q=0.5,en;q=0.3',
            'Upgrade-Insecure-Requests': '1',
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:55.0) Gecko/20100101 Firefox/55.0',
        }

        self.name='I Want My Tears Back'

    def run(self,sn):
        self.name=sn
        session=requests.session()
        firstjsonurl='https://c.y.qq.com/splcloud/fcgi-bin/smartbox_new.fcg?is_xml=0&format=jsonp&key={}&g_tk=5381&jsonpCallback=SmartboxKeysCallbackmod_top_search1467&loginUin=0&hostUin=0&format=jsonp&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq&needNewCode=0'.format(self.name)
        r=session.get(firstjsonurl).text
        print(type(r))
        print(r[39:-1:])
        myjson=json.loads(r[39:-1:])
        mid=myjson['data']['song']['itemlist'][0]['mid']
        print(mid)
        searchurl='''https://u.y.qq.com/cgi-bin/musicu.fcg?callback=getplaysongvkey2236996910208997&g_tk=5381&jsonpCallback=getplaysongvkey2236996910208997&loginUin=0&hostUin=0&format=jsonp&inCharset=utf8&outCharset=utf-8&notice=0&platform=yqq&needNewCode=0&data={"req":{"module":"CDN.SrfCdnDispatchServer","method":"GetCdnDispatch","param":{"guid":"8665097290","calltype":0,"userip":""}},"req_0":{"module":"vkey.GetVkeyServer","method":"CgiGetVkey","param":{"guid":"8665097290","songmid":["'''+mid+'''"],"songtype":[0],"uin":"0","loginflag":1,"platform":"20"}},"comm":{"uin":0,"format":"json","ct":20,"cv":0}}'''
        r=session.get(searchurl).text
        print(r)
        songjson=json.loads(r[32:-1:])
        print(songjson)
        header=songjson['req_0']['data']['sip'][0]
        two=songjson['req_0']['data']['midurlinfo'][0]['purl']
        songurl=header+two
        with open("{}.mp3".format(self.name),'wb') as ms:
            print(songurl)
            raw = session.get(songurl, headers=self.headers)
            content=raw.content
            if len(content) >500:
                ms.write(content)
                print("下载成功")
            else:
                print("下载失败")

App=downloadMusic()
while 1:
    songName=input("请输入歌曲名字")
    App.run(songName)
```
效果如下：

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-29/QQ音乐MP3下载/1553846610023.png)



<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>