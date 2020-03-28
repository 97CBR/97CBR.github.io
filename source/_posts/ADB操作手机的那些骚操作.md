---
title: ADB操作手机的那些骚操作
tags:
  - ADB
  - Android
  - 有趣的东西
categories: Android
keywords:
  - ADB操作手机
  - ADB
  - 有趣的东西
description: ADB连接手机的那些姿势
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-28/BogImages/1585373940098.png
abbrlink: c051e6bd
date: 2019-04-19 00:00:00
---

## 前文再续，书接上一回

承接上一篇 ADB连接手机的那些姿势 ，我们已经成功地将用ADB与手机连接上，无论是有线亦或是无线连接。

这个时候，我们就可以使用ADB操作手机了。

## 确认设备状态

我们先确认一下设备有没在线先。

- adb devices
![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555637982324.png)

那么我们再测试一下手机有没root权限吧。

``` shell
adb root
adb shell
```


输入adb root后，会重启一下adb，然后输入adb shell后，观察前面的命令提示符。如果变成了#就说明提升为root权限成功了，否则就是未root的手机。
对于那部分成功进入root 模式的同学，可以用 **adb unroot** 回到正常状态

## 我们还可以用adb来安装、卸载、启动、某个应用

### adb启动应用

``` shell
adb shell
pm list packages
```

- adb shell 进入shell中
- pm list packages 列出所有应用App

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555638499340.png)

有些同学就会发现App太多找来找去都找不到自己要找的那个App
这个时候，我们就可以用 | grep 来过滤我们的关键字了，比如

``` shell
 pm list packages | grep camera
```

前面那句就是列出所有package，然后用grep 过滤 含camera 的包

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555638756745.png)

那我们就启动OPPO自带的相机吧。
``` shell
 am start -n com.oppo.camera/.Camera 启动相机
 am start -n  com.tencent.mobileqq/.activity.SplashActivity 启动QQ
```

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555639272947.png)

效果如下：
![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/启动相机3.gif)

 类似的，我们还可以用 am -start 启动其他的软件，例如	QQ、微信什么的。

 我们还可以找到软件的安装路径

``` shell
adb shell pm path com.tencent.qqmusic
```

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555640677754.png)

 ### adb卸载、安装 应用
 adb卸载以及安装的命令对应为
``` shell
adb uninstall 包名
adb install apk路径
```

 卸载腾讯视频
![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555640880140.png)

 安装QQ音乐
![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555641252739.png)

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555641273242.png)


## adb 传输文件

- adb push pc文件路径 设备路径
 如 adb push C:\Users\Marx\Desktop\date.jpg /sdcard/
![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555641574521.png)
- adb pull 设备路径  pc文件路径

``` shell
adb pull /data/app/com.tencent.qqmusic-P2749Smc9nccljwLiexzJA==/base.apk C:\Users\Marx\Desktop
```

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555641121514.png)

## adb 模拟操作

我们先进入到 adb shell 中
- adb shell
- input  ，你会看到很多提示
- ![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555641761038.png)
-  input tap 700 700 ，将会在 700,700 的位置点击一下
![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555641999513.png)

## adb 实现拍照

在我现在打开的相机的场景中，那我们怎么点击拍照呢？我们并不知道拍照按钮的位置哦。又只能靠猜吗？
显然不是，我们有两个办法。

 比如我们的场景是在相机里面点击拍摄，我们可以用到keyevent来操作
- input keyevent 27 是拍照的指令，但是这个时候要你打开相机

 更多的keyevent可以进入到这个网址 查看 https://developer.android.com/reference/android/view/KeyEvent.html
 如果进入不了的同学，可以后台回复 keyevent ，获取keyevent的文档。
我在这里挑几个好玩的出来给大家随意参考一下。
- 3	HOME 键
- 4	返回键
- 26	电源键
- 82	菜单键

## adb 实现截屏

``` shell
- adb shell /system/bin/screencap -p /sdcard/screenshot.png
- adb pull /sdcard/screenshot.png d:/screenshot.png
```

- adb shell /system/bin/screencap -p /sdcard/screenshot.png ， 截屏到 sdcard
- adb pull /sdcard/screenshot.png d:/screenshot.png ，传到电脑中

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555645470814.png)

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555645411272.png)
 ## adb 查看系统信息
 - 查看电池信息 dumpsys battery
![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555643526178.png)

- 屏幕分辨率 wm size

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555643663082.png)
- 查看网络信息 ifconfig
 ![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555643926200.png)

- 查看Android的版本 getprop ro.build.version.release
 ![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555643804691.png)

- 查看内容以及资源使用情况 top，为了显示明显一点，打开了“王者”，大家可以看到最上面那个应用所占的资源。

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555644338587.png)

## adb查看应用日志

-  adb logcat 这个简直学过Android的同学必备技能了。
-  adb logcat \*:E ，输出所以 错误日志

![default](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-19/ADB操作手机的那些骚操作/1555644813865.png)

还有很多很多的过滤以及对应进程查看的操作，大家自行查阅资料，这里便不展开了。

因为Android是一个基于Linux内核的开放源代码移动操作系统。所以很多Linux命令在adb shell 中也是适用的。我在这里就不一一说明了。例如 cat、cd、chmod、df、grep、kill、mv、之类的命令，大家可以自行去查资料。

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>