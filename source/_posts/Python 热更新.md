---
title: 软件更新之Python热更新
tags:
  - Python
  - 有趣的东西
  - 热更新
categories: Python
keywords:
  - Python
  - 热更新
  - 有趣的玩意
description: 软件更新之Python热更新怎么搞
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-28/BogImages/1585331007993.png
abbrlink: 654b738c
date: 2019-06-05 00:00:00
---

## 前言
本篇文章涉及技术知识如下：
> - Redis
> - threading
> - 多线程
> - PyQt5
> - importlib
> - 热更新

## 热更新的场景
咱们在平时运行一些长时间都会一直运行的软件（如：某些云同步软件）的时候，某些功能因为考虑的情况可能不充分，导致体验不够好的时候，很多人都会忽视这个问题，除非这个问题影响到他正常使用了。但是也有部分用户会在软件的反馈框里面将问题反馈给开发者，顺带将错误日志也一并提交给开发者。然后过了一天或者半天，你再运行那部分功能的时候，发现问题已经解决了。可是，我们都没有更新软件呀，甚至连软件都没有重启，难道前面遇到的那个情况真的是因为自己太幸运踩中bug了吗？
其实，我们之前遇到的问题，可能的确就是一个bug，但是在反馈问题给开发者后，开发者快速定位问题所在后，通过热更新将问题解决了。相当于我们使用的软件自动fix了一些bug，更新了一次版本。
那么，今天咱们聊一下热更新这个东西怎么样？我们也随意做个小demo看看这个有意思的功能是怎么做到的。

## 什么是热更新
热更新就是可以在进程不重启的情况下，让其重新加载修改后的程序代码，且能按照预期正确执行。在实际开发中，热更新的最主要用途有，
- 可以提升开发效率，让改动后的代码效果立刻实现，避免频繁重启
- 对于bug修复来说，在CS模式下，如果不是大的bug而是小bug的修复就不用发布比较大的补丁和更新文件了，直接使用服务器修正问题后，通知客户端重新加载修正后代码即可。

## Python如何进行热更新

Python的代码是通过module进行组织的，所以，对某些功能的热更新就是可以通过对module更新就可以了。
在Python中，如果重新import 一个已经被import的模块时，并不会重新执行import新的模块。所以，在这个时候，我们希望可以重新加载模块的时候，就需要对模块进行删除后，再重新import进来。
而在sys.modules保存了已经加载过的模块。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-13/Python_热更新/1555127410118.png)

## 利用之前的客户端进行展示效果

为了方便看到展示，我就沿用上次客户端的界面，进行简单修改后，展示给大家看，热更新的效果。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-13/Python_热更新/1555127515855.png)

左边的按钮是运行模块加载进来的函数，右边的按钮是手动点一下热更新。便于本地手动调试热更新。在后面实现的“发布订阅”情况中，服务端发布更新消息后，不用手动点 热更新 就可以对软件进行自动更新了。

## 测试思路及效果

简单实现一个demo，引用myfunction这个模块，运行里面的某个函数一两次后，修改那个被运行的函数实现，然后对myfunction这个模块进行热更新，看看效果怎么样?

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-13/Python_热更新/1555086396180.png)

在热更新前，随机产生的数字在原函数里面，版本号为0.0.1，是可以比较明显看出 两个数 是运行 “相加” 操作的。
点击了热更新Button后，软件并未重启，在更新后，可以 看到功能版本号发生了改变，变成了0.1.1，说明已经是热更新完成了的。再点击运行功能，可以看到结果已经变成了 两个数字 进行了 “相减‘ 操作。

## 着手实现 CS模式下 的自动热更新

完成了本地测试热更新成功后，就着手实现CS模式下的“发布订阅”消息通知功能，利用服务器对客户端推送一个更新指令，客户端就会自动更新模块。

### 利用Redis的发布订阅模型

用过Redis的同学应该都知道Redis本身就自带了“发布订阅”功能，借助它，可以很方便的实现出远程推送消息的需求，我们甚至可以用这个功能实现一个简单的聊天室软件哦。
在Redis服务端中，创建一个 update频道：

>SUBSCRIBE update

### 客户端修改，使用线程避免阻塞

然后在Python中导入Redis模块后，链接到远程Redis数据库后，订阅我们的update频道，再启动一个新的线程去监听update频道的消息。
因为如果直接在代码里面用单线程去监听消息的话，会造成线程阻塞在监听消息哪里，导致界面刷新不出来。所以，我们只要导入threading库，再把监听消息做成一个函数，放到thread中去运行就可以了。由此避免线程阻塞问题。

### 发送更新指令
接着我在本地修改一下myfunction模块后，就到Redis服务的终端中，发布一个消息，reload。
这个时候，软件就会收到reload消息，对刚才被我修改后的模块进行热更新，即删掉源模块，再重新导入一次。

在这里我就不写一次从服务器中下载新的模块文件的代码了，假设我刚才修改后的那个文件就从服务器下载下来的。同学们可以借助前面两篇写软件更新服务的文章来自己实现一个文件下载更新的代码。很简单的，只要你愿意写。

在Redis中发布重载的消息后，订阅了这个频道的客户端，将会接收到更新信息，比如我这个客户端，将会对模块重新加载进行热更新了。

在这里我给大家随意扯一下“灰度测试”吧，这个灰度测试就是软件即将要更新某个功能，但是可能这个功能还不够稳定，不能向全部用户推送新的功能。所以，这个时候，就需要对部分用户更新这个功能，通过这部分用户使用情况来决定灰度测试的范围，比如将5%的范围扩展到10%这样。
至于怎么筛选出那些用户为测试用户呢？其中有个办法是可以用个哈希函数对用户某个值，如用户名，进行处理，符合的就推送。当然还有很多很多的策略，实现执行起来的时候，也不会像我说的那么简单，感兴趣的同学可以自行查阅资料。

### 利用Redis发送更新消息

接下来，我们来测试一下发布更新功能的消息后，有没正常热更新功能。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-13/Python_热更新/1555088922933.png)


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-13/Python_热更新/1555088948860.png)

在这里要提醒一下，如果你在热更新前导入的模块生成了一个对象x，这个时候，你热更新了，然后又生成一个对象y。这个时候，你会发现，x指向的仍旧是旧的那个类，而y则指向了新的类。这个时候，可以通过修改x的__class__属性来对 x 的类进行强制修改，可以这样写：
>x.__class__ == y.你的类

但是即使你是这样写，你x里面的数据仍旧不会发生改变的哦。我们只更改了代码的执行逻辑。

## 代码呈上

代码放这里了：

``` python
# -*- coding: utf-8 -*-
# @Time    : 4/12/2019 14:02
# @Author  : MARX·CBR
# @File    : __init__.py.py
import threading
import sys
from PyQt5 import QtWidgets
from updateServer.HotUpdate import myfunction
import redis
import random
import importlib
from updateServer.HotUpdate.HotFixSample import Ui_MainWindow


class MainWindow(QtWidgets.QMainWindow, Ui_MainWindow):
    def __init__(self, parent=None):
        super(MainWindow, self).__init__(parent)
        self.setupUi(self)
        self.fun = importlib.import_module("myfunction")
        self.pushButton.clicked.connect(self.runFunction)
        self.pushButton_2.clicked.connect(self.hotfix)
        self.ip = "47.xxx.xxx.xx"
        self.redisport = 2017
        self.redis_manager = redis.StrictRedis(self.ip, port=self.redisport)
        # self.textBrowser.append(str(sys.modules))
        print(sys.modules)
        self.tunnel = self.redis_manager.pubsub()
        self.tunnel.subscribe(["update"])
        self.threads = []
        self.t1 = threading.Thread(target=self.autoReload, )
        self.threads.append(self.t1)
        self.threads[0].setDaemon(True)
        self.threads[0].start()

    def autoReload(self):

        for k in self.tunnel.listen():
            if k.get('data') == b'reload':
                self.hotfix()

    def runFunction(self):
        version = self.fun.AllFunction().version
        self.textBrowser.append("功能运行，当前版本为：" + version)
        for i in range(4):
            x = random.randint(-454, 994)
            y = random.randint(-245, 437)
            self.textBrowser.append(str(x) + "\tfunction version {}\t".format(version) + str(y) + " = " + str(
                self.fun.AllFunction().second(x, y)))
        # self.textBrowser.append(self.fun.AllFunction().first())

    def hotfix(self):
        del sys.modules["myfunction"]
        self.fun = importlib.import_module('myfunction')
        self.textBrowser.append("热更新完毕")


if __name__ == "__main__":
    app = QtWidgets.QApplication(sys.argv)
    mainWindow = MainWindow()
    mainWindow.show()
    sys.exit(app.exec_())
```

## 总结

在本篇文章中，介绍到了Python的热更新的一些简单用法。以及一些值得注意的坑。顺手应用了Redis的“发布订阅”功能来通知客户端更新功能。本篇文章代码已同步上传到我GitHub中，欢迎大家fork使用，顺手给个star就更好了，项目链接  [SoftwareUpdateServer](https://github.com/97CBR/SoftwareUpdateServer)

其实，有心思的小伙伴肯定会有更多的想法。比如：我们既然可以动态重新加载一个类来fix bug，也肯定可以动态添加我们要的功能啦。这意味着，我们可以编写出一个软件，具有插件功能的软件。在主体软件上面，运行插件来扩展更加多的功能，和Chrome这样的浏览器一样，安装插件什么的。

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>