---
title: Python分析聊天记录
tags:
  - Python
  - 数据可视化
  - 有趣的玩意
categories: Python
keywords:
  - 数据可视化
  - 有趣的玩意
description: 用Python分析一下和妹纸聊天的内容
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574474939006.png
abbrlink: 318b512
date: 2019-11-23 00:00:00
---

## 跟妹纸聊天是每个男孩纸的梦想
![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574477164015.png)

同学们，许久不更新了~
内容嘛~如题所示，用Python来小小的分析一下和妹纸的聊天记录。
分析看看和她聊天互发了多少条消息，用到最频繁的词语是什么，最高频发送消息的时间等等。

PS：本文的数据展示已取得对方的同意~

## 导出聊天记录

首先，和她最主要的沟通工具是Tencent家的QQ，而比较少用的微信，就不作统计了。（PS：我差点找不到导出聊天记录的方法）
所以，为了节省大家的时间，给大家简明扼要的说说怎么导出聊天记录（方法不唯一，仅供参考）

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574477616623.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574477651780.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574477726627.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574477778311.png)

到这里就已经是导出聊天记录了

## 准备写一手代码

咱们将聊天记录放到咱们的代码目录里面去，Python用起来

### 先统计一下我跟妹纸互相发送了多少条消息

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574478319903.png)

这个数据的计算很简单，根据导出的聊天记录观察一下就可以知道要怎么统计了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574478422876.png)

如图，咱们只要统计用户名，备注名就可以了。其他不包含名字的，就不统计即可

### 咱们统计一下，互相发送最高频的词

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574476316773.png)

这是柱状图，横轴就是词语，纵轴自然懂

这个数据的来源，是利用了最多人使用的jieba分词的库进行统计的
既然利用别人的库进行词语划分，所以我们要处理的还是，数据的筛选的问题。
所以，关注点是怎么去掉不希望统计到的内容，比如用户名、备注名、表情包、图片等等内容。
这里，还是上面统计消息次数那招，看到有不希望看到的，就不要了。
所以，可以用一段小函数，去过滤掉即可

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574478941833.png)

这样写，虽然蠢，但是实现够快速呀~（有时间写这么多高级的算法，还不如多陪陪妹纸？）

### 再统计发送消息的散点图

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574476564829.png)

这是2D的散点图，纵轴是0~23小时、横轴是分钟，后面的数字都黏在一起了。。。。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574476581634.png)

### 2维的不够骚看看3维的

这个图是3D的散点图，横轴是0\~60分钟，纵轴是0\~23小时，竖轴是0\~60秒

为啥要做2D和3D的两个图出来呢？因为一分钟内发送的N条消息，无法在上面那个2D的图中展示出来，而3D的就可以将，这几千条消息都展示出来了。（当然，因为没考虑日期的问题，某些点还是有可能会重复叠在一起）

从这两个图，就可以大概看到发送消息的高频时间段以及低频的时间段，以及某些特别的点 (如凌晨5点钟的消息~)

## 生成词云

最后，就是题图的生成了，题图是一个词云。使用的是 wordcloud 这个库，简单快速的生成了一个词云。
可以生成带图案模型的图片哦，我这里这张用的就是那张，张开手的乔巴 。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-11-23/Python分析聊天记录/1574474939006.png)

好了，扯了这么多，不给一份代码，感觉都有点奇怪。应该结合代码来写的，这篇内容。
（脑洞：把要说的东西，都在代码里面用注释写？）

代码就贴这里吧（超烂勿喷）：
``` python
# -*- coding: utf-8 -*-
# @Time    : 11/20/2019 5:29 PM
# @Author  : MARX·CBR
# @File    : chat_analysis.py

import jieba
import numpy as np
import matplotlib.pyplot as plt
from plotly.offline.offline import matplotlib
from mpl_toolkits.mplot3d import Axes3D
from wordcloud import WordCloud, ImageColorGenerator
from PIL import Image

boy_name = "MARX·CBR"
girl_name = "xxx"


def count_num(content):
    marx_count = 0
    hr_count = 0
    for li in content:
        if boy_name in li:
            marx_count += 1
        elif girl_name in li:
            hr_count += 1
    print("{} sended : {}".format(boy_name, marx_count))
    print("{} sended      : {}".format(girl_name, hr_count))


def use_jieba_and_show(content):
    words = jieba.lcut(content)
    # print(words)
    counts = {}
    for word in words:
        if len(word) == 1:
            continue
        else:
            counts[word] = counts.get(word, 0) + 1

    items = list(counts.items())
    items.sort(key=lambda x: x[1], reverse=True)

    # 把注释去了，便可看到柱形图

    # lab = []
    # num = []
    for it in items[:20:]:
        word, count = it
        print("{}\t{}".format(word, count))
    #     lab.append(word)
    #     num.append(count)
    # plt.bar(range(len(lab)), num, tick_label=lab)
    # plt.show()


def remove_pic(content):
    if "[图片]" in content:
        content = content.replace('[图片]', '')
    elif "[表情]" in content:
        content = content.replace('[表情]', '')
    return content


def show_more_word(content):
    all_msg = ""
    for line in content:
        if boy_name in line:
            all_msg += " "
        elif girl_name in line:
            all_msg += " "
        else:
            msg = remove_pic(line)
            all_msg += msg
    use_jieba_and_show(all_msg)


def show_times_3d(all_msg_time):
    fig = plt.figure()
    ax = plt.subplot(projection='3d')
    X = np.arange(0, 60, 1)
    Y = np.arange(0, 24, 1)
    plt.xlim((0, 60))
    plt.ylim((0, 24))
    plt.xticks(X, fontsize=10)
    plt.yticks(Y, fontsize=10)

    for ti in all_msg_time:
        try:
            y = int(ti[0:2:])
            x = int(ti[3:5:])
            z = int(ti[6:8:])
            ax.scatter(x, y, z, s=2)
        except:
            ...
    #    plt.show()
    ax.set_zlabel('second')
    ax.set_ylabel('hour')
    ax.set_xlabel('minute')
    plt.savefig('chat_3d.png', dpi=300)


def show_times_2d(all_msg_time):
    fig = plt.figure()
    ax = plt.subplot()
    my_x_ticks = np.arange(0, 60, 1)
    my_y_ticks = np.arange(0, 24, 1)
    plt.xticks(my_x_ticks)
    plt.yticks(my_y_ticks)

    for ti in all_msg_time:
        try:
            y = int(ti[0:2:])

            x = int(ti[3:5:])

            ax.scatter(x, y, s=5, alpha=0.9)
        except:
            ...
    #    plt.show()
    plt.savefig('chat_2d.png', dpi=300)


def show_times(content):
    all_msg_time = []
    for li in content:
        ct = ""
        if boy_name in li:
            ct = li[11:19:]

        elif girl_name in li:
            ct = li[11:19:]
        if "" != ct:
            all_msg_time.append(ct)
        else:
            ...
    all_msg_time.sort()
    print(all_msg_time)
    show_times_2d(all_msg_time)
    show_times_3d(all_msg_time)


def generation_cloud_img(content, filename):
    path_img = "./background.jpg"
    background_image = np.array(Image.open(path_img))
    cut_text = " ".join(jieba.cut(content))

    wordCloud = WordCloud(font_path="C:/Windows/Fonts/simfang.ttf",
                          background_color="white",
                          mask=background_image,
                          width=1920,
                          height=1080).generate(cut_text)
    image_colors = ImageColorGenerator(background_image)

    plt.imshow(wordCloud.recolor(color_func=image_colors), interpolation="bilinear")
    # plt.imshow(wordCloud,interpolation="bilinear")
    plt.axis("off")
    # plt.show()
    plt.savefig('{}'.format(filename), dpi=300)


def cloud_img(content):
    # path_img = "./backgrounds.jpg"
    # path_img = "./hrs.jpg"
    # path_img = "./rrby.jfif"
    all_msg = ""
    for line in content:
        if boy_name in line:
            all_msg += " "
        elif girl_name in line:
            all_msg += " "
        else:
            msg = remove_pic(line)
            all_msg += msg
    generation_cloud_img(all_msg, "cloudword.png")


def separate_analysis(content):
    cbr_msg = ""
    hr_msg = ""
    flag = 1
    for line in content:
        if boy_name in line:
            flag = 1
            continue
        elif girl_name in line:
            flag = 2
            continue
        else:
            if flag == 1:
                msg = remove_pic(line)
                cbr_msg += msg
            elif flag == 2:
                msg = remove_pic(line)
                hr_msg += msg
    use_jieba_and_show(cbr_msg)
    use_jieba_and_show(hr_msg)
    generation_cloud_img(cbr_msg, 'cbr.png')
    generation_cloud_img(hr_msg, 'hr.png')


if __name__ == '__main__':
    print('Run it ~')
    # chat.txt 就是聊天记录
    with open('xxx.txt', 'r', encoding='utf-8') as f:
        contents = f.readlines()

    # count_num(contents)
    # show_more_word(contents)
    # show_times(contents)
    # cloud_img(contents)
    separate_analysis(contents)

```

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>

