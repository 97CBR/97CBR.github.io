---
title: WO麦克风软件破解收费功能以及去广告
tags:
  - APK
  - 逆向
  - 破解
categories: 信息安全
keywords:
  - 破解
  - 信息安全
  - 逆向
description: 利用反编译工具对APK进行逆向分析破解
cover: https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-24/BogImages/1585059107768.png
abbrlink: 9b7bb0cb
date: 2019-10-23 00:00:00
---

## 没有录音插孔的烦恼

还是先废话说说怎么会想到做这个软件的破解。跟大家扯一下背景，交代一下下防止大家有疑问。因为我的Lenovo小新只有一个耳机输出口，没办法用到我耳机上面的麦克风，只能用本子自带的那个麦克风。这就导致了平时**录音质量不好**（其实是因为开黑的时候语音被吐槽说话太小声了）。

## 找找有没便宜简单的解决方案

所以问了问谷歌、度娘什么的，就晓得了“WO麦克风”这个软件。然后便安装好PC的客户端以及Android的客户端。在局域网内使用WIFI连接，测试了一下，收音效果比笔记本自带的mic好些，延迟也在可接受范围。

但是，在Android客户端有一些小瑕疵，比如：有广告、音量无法调整。尝试调整音量时，就提示我这个音量调整是收费功能，订阅即可\~
Android客户端大概长这样子

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解以及去广告/1571820216731.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解以及去广告/1571820187080.png)

## 穷屌丝没钱订阅，试试破解

先取回apk文件

这里因为本人不想拿根数据线连着手机，太麻烦了。所以选择用adb 无线连接过去进行操作。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/WO录音软件修改/1571664370249.png)
使用默认端口连接

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/WO录音软件修改/1571664324027.png)
将我们需要修改的这个wo麦克风取回电脑本地

有同学可能不知道我是怎么找到这个软件的路径取回来的

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/WO录音软件修改/1571664352302.png)
怎么做找到这几个软件的路径？进到adb shell里面再使用一个 pm path 命令很简单啦
蛤包名不知道咋个找到？请回顾前面发过的文章“ADB操作手机的那些骚操作”，不赘述啦~

## 逆向分析

取回来之后，右键apk文件，用压缩软件打开瞅一眼，如下所示

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解以及去广告/1571821623983.png)
进入res目录没看到什么奇奇怪怪so文件，咱们就先假设他没壳没加固过

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/WO录音软件修改/1571665564118.png)

### 进行反编译
将apk文件丢进APKIDE（APK改之理）中，自动反编译便会安排上了~
当然，有可能有些同学会反编译失败。出现类似这样的东西\~

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571822362719.png)
明显就是告诉你，反编译错误了

### 更换apktool
这个情况，给他换一下apktool就好了

下载一个新的apktool回来，将原来那个换掉就好。你看原来自带的apktool时间戳都2015年的，安卓发展这么快，反编译后面新做的软件难免会出问题的嘛。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571822444921.png)
OK，反编译好，打开清单文件就可以看到这样的界面了。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571822707756.png)

### 找找广告
咱们开头说了，不喜欢广告，我们在AndroidManifest里面看看能不能找到一些关于广告的东西

其实都不用怎么看，扫视了一眼，就发现了关于广告的内容。因为看到了ads，咱们平时有同学用过屏蔽网页广告的软件吧，是不是也带有ad

谷歌提供的广告服务，com.google.android.gms.ads.APPLICATION_ID

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571822875717.png)

ads是谷歌公司的主要广告服务产品\~
咱们前面说了，Android软件就是由一个个activity做成的。所以，咱们在这里把有关ads的activity都删掉试试看，能不能编译出来，能否去掉广告\~
在这里，搜索一下关于ads的所有内容，并把所在行标记好

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解以及去广告/1571820613324.png)
## 修完完毕重新编译
OK，这三行，咱们都给删了，然后保存。编译一下\~

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/WO录音软件修改/1571668660546.png)
报错失败了，咱就知道不会这么简单\~

### 找到报错点
看一下报错，是资源错误，这就很好办了\~去报错所在xml，找到这个属性，删掉保存就好\~


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-21/WO录音软件修改/1571668686268.png)
编译，安装，保存按钮灰色说明已保存\~

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571823433432.png)
OK,删掉之后，确认保存更改之后，咱们再编译一次。
编译通过了

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571823538688.png)

## 编译通过，准备重新安装

点击安装按钮\~

如果安装失败~，把手机原来的软件卸载掉再试一次，想知道为啥要这样的同学，麻烦回顾以前发过的文章\~
### 看看去广告后的效果
OK，装好之后，打开软件，可以看到，已经是没有广告了\~

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-29/WO麦克风软件破解收费功能以及去广告/1572324077218.png)


但是，录音的音量还是调整不了。。。

## 重新分析，尝试破解音量调节


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571825976390.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571826021533.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571826035485.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571826055537.png)
 看看Java代码

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571826126626.png)
是这里没错了，结合函数名推测一下\~

不妨在这里看看，看一下这段函数，写了啥

``` java
   if (this.mSettings.isSubscriptionValid())
		{
		  this.mSettings.setVolume(paramInt);
		  if (this.mBinder != null) {
			this.mBinder.setVolumeLevel(paramInt);
		  }
    }
```

这段是设置音量的，咋一看没啥问题。在if语句判断一下是不是通过订阅，如果通过的话，进入循环体使用setVolume来设置音量

``` java
    do
    {
      return;
      this.mSettings.setVolume(5);
      this.mVolumeBar.setProgress(5);
    } while (this.mConfirmingToSubscribe);
    confirm("", "Volume adjustment is a premium feature. You need to subscribe to do that.", "Subscribe", "Cancel", "subscribe");
    this.mConfirmingToSubscribe = true;
```

这个do循环，进去就给return 了\~有点迷。。。不过，似乎是为了锁住音量条的

OK，咱们就从this.mSettings.isSubscriptionValid() 入手，看看这个方法是啥东西

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571827778193.png)

点击这个函数，自动跳转过去

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571827830485.png)

再跟一下mIsSubscriptionValid

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571827873145.png)

可以看到在 Settings 这个类里面声明了这个变量
双击它，看看这个类里面还有什么地方用到了这个变量

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571828150020.png)

这个地方，看起来像给他赋值一个false啊

好，不如就先给他把false改成true试试
当然在这里Java伪代码里面改是木得用的，要去smali里面去改

找到这个settings.class文件的smali代码

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571829032576.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO麦克风软件破解收费功能以及去广告/1571829068564.png)

这些smali代码对照着Java，是不是很容易就定位出这个文件没错了。
找到咱们要改的subscriptionValid

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO录音软件修改/1571811145227.png)

伪代码里面有这个getBoolean出现吧\~，咱们看看这行写的啥
    invoke-interface {v2, v3, v5}, Landroid/content/SharedPreferences;->getBoolean(Ljava/lang/String;Z)Z

invoke-interface 是个调用指令，调用了android/content/SharedPreferences类里面的getBoolean，传了V2,V3,V5 这三个参数。根据Java伪代码以及经验，咱们v5存的就是false了

往上面找\~看看v5赋了啥进去

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO录音软件修改/1571811163258.png)
给了个0x0，就是0的意思咯，众所周知0就是false噻
咱们把他整成0x1，就是true了吧

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-23/WO录音软件修改/1571811154922.png)

## 重新回编译安装

OK，改完，保存好。编译，安装一下。测试

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-29/WO麦克风软件破解收费功能以及去广告/1572326180719.png)

可以看到已经被我们随意调节音乐大小了
鉴于实在不方便在文章里面展示出调节收录音量大小的变化，就不作演示了。但是，亲测是可以去掉广告，正常使用并可以正常调节音量的哟。

## 附录：测试平台
测试平台：Redmi Note 3

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-10-29/WO麦克风软件破解收费功能以及去广告/1572326432466.png)
逆向平台：Windows 10 专业版、APKIDE 3.2.0.0、akptool 241


## 总结一下
  1、Android的逆向分析，先判断有没加壳（有的话就先不做了，不在咱们现在这篇文章的讨论范围内）

   2、然后丢进反编译工具处理，尽量把Java的代码也反编译出来，方便自己对代码的理解。毕竟人肉解析smali代码还是有点儿难受的~

   3、根据自己要修改的内容去找到对应的smali文件，看一下他的处理流程，结合Java代码看会好些

   4、确定修改点之后，就修改文件。修改好一点就编译测试一下，别贪多，一步一步排查，心急吃不了热豆腐嘛~

   5、编译安装之后，检查App以及功能是否还能正常使用



## 结尾叨叨


   又到结尾叨叨的时候了，这次这个文章也是2400字左右（基本是三篇高中作文的量？），读到这里的同学们仍旧是真爱~

   Android逆向是比较有意思的一个东西，有一点儿门槛。随着Android软件的安全攻防推进，现在的App大部分都做的比以前安全多了。所以，逆向破解难度也在逐步提高，这是好事。

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>