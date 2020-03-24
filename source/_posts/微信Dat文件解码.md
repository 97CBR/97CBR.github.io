---
title: 微信Dat文件解码
tags:
  - 破解
  - 微信
  - Python
categories: 工具
keywords:
  - 破解
  - 微信
description: 利用Python对微信缓存的DAT文件进行解码
cover: https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-24/BogImages/1585032114547.png
abbrlink: 36cb7ea6
date: 2019-03-28 00:00:00
---

## 有空总得收拾一下下自己的本本

最近在整理磁盘文件，因为经过一段时间的蹂躏后，磁盘实在是太多东西了，不整理一下，简直对不住我的SSD好嘛。偶然发现磁盘中某公司的文件夹占用空间简直不能再大，那可是我的C盘啊，合计才119GB的SSD空间，你给我占了差不多10个G，说的就是你Tencent。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-28/微信Dat文件解码/1553763845852.png)

但是也不能怪人家，毕竟人家只是负责将数据保存下来方便给我们展示而已。所以，就冒着好奇的心，看看文件夹里面有什么。因为这次的主题说的是Dat文件的解密，我就不扯那么远。 大家假装知道我点完文件夹，看到很多历史图片缓存图片众多*.db就好了哈。（PS：不得不提的是腾讯家的QQ就将近给我缓存了7个G的表情包，未来可以收集这个内容做一个随机表情包网页出来乐呵乐呵呀。咳咳这些都是后话啦）

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-24/BogImages/1585032348049.png)

然后我们便来到了微信PC版的文件夹，找到自己的账号所在文件夹。找到后，如下所示

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-28/微信Dat文件解码/1553763834931.png)

## 让我来猜猜里面的东西

这个时候，我便猜测，这些dat文件都是什么内容呢？聊天内容？不可能呀，聊天内容这么机密，肯定是放到db里面加密处理的。聊天文件？也不至于有1700+个文件吧。所以，综上，猜测这些个dat文件都是一些聊天时接收到的“表情包”或者“图片”。嗯，平时那些群聊斗图这么凶，估计没错了的。
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-28/微信Dat文件解码/1553763812076.png)

接下来，尝试直接改后缀试试。不出意外的得到了“图片错误”，我就知道不会这么简单。。。
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-28/微信Dat文件解码/1553763909684.png)
### 不如试试异或加密
那接下来怎么办嘛。思来想去，便想着拿16进制编辑器看一下下，里面数据长啥样。打开了多个文件看到里面文件头是8A AD ，但是一般jpg文件的文件头为FF D8 开头的。又记得之前看过说文通过异或对文件进行简单的加解密的很常规的做法。所以，打开计算器。一顿操作猛如虎，哈哈哈，结果一看没错辽，
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-28/微信Dat文件解码/1553763921427.png)

将计算器调整到程序员模式，然后使用FFD8与8AAD进行异或处理。结果为7575，显然可以知道，这个dat就是微信将收取到的文件对吗，每个字节进行异或0x75进行加密，保存为dat文件。
这个时候，只要用Python写个脚本，不就可以轻松解码了嘛。

## 那就写一手代码试试？



``` python
# -*- coding: utf-8 -*-
# @Time    : 3/27/2019 21:54
# @Author  : MARX·CBR
# @File    : 微信Dat文件转图片.py

import os

s_out_path = r"D:\WeChat_Decode\\"

def image_decode(f, fn, dest_floder):
    dat_read = open(f, "rb")
    # out='P:\\'+fn+".png"
    out = dest_floder + fn + ".jpg"
    print(out)
    png_write = open(out, "wb")
    for now in dat_read:
        for nowByte in now:
            newByte = nowByte ^ 0x75
            png_write.write(bytes([newByte]))
    dat_read.close()
    png_write.close()


def findFile(f, dest_floder):
    fsinfo = os.listdir(f)
    for fn in fsinfo:
        temp_path = os.path.join(f, fn)
        if not os.path.isdir(temp_path):
            print('文件路径: {}'.format(temp_path))
            print(fn)
            image_decode(temp_path, fn, dest_floder)
        else:
            ...


running_path = r"F:\XXX\WeChat Files\XXX\Data\\"
findFile(running_path, s_out_path)
```
跑出来的结果如下图，没错吧，以往的表情包，发过收过的图片都出现在了里面。
至此微信dat文件的复原解码到此就完成了。补充一下，每个账号或者客户端对那个异或值都是不一样的，所以，小伙伴们如果需要还原dat文件的话，还是要自己用计算器异或处理一波哦。看看这个值是多少哈。

### 代码实现效果如下

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-3-28/微信Dat文件解码/1553763936147.png)



<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>