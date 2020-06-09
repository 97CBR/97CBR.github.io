---
title: PYQT5开发环境搭建
tags:
  - QT
  - Python
  - 界面开发
keywords:
  - QT
  - 界面开发
categories:
  - - QT开发
  - - Python
description: PYQT5开发环境搭建
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-5-19/BogImages/1589856479947.png
abbrlink: 82219c25
date: 2019-04-26 11:57:00
---

## 前言
今天看看如何搭建基于Python的PyQt5开发环境的搭建。

俗话有说：砌墙先打基，吃蛋先养鸡。我们要用Python做界面开发的话，就得把开发环境给搭建好才可以进行。下面，请跟着我的步伐，将你的开发环境搞起来，可好？（搞的像真的一样。。。）

## 步骤一览

- 第一步，把Python安装上
- 第二步，把IDE安装上
- 第三步，把依赖包安装上
- 第四步，添加外部设计工具
- 第五步，设置ui转py文件工具
- 第六步，设置资源转py文件工具
- 第七步，引用自动生成的界面代码，运行

呐，不多不少，就简简单单的7步，就可以将PYQT的开发环境给搭建好了。

## 安装好Python

第一步：装python ，到Python的官网去下载即可。本人推荐Python36。
链接在此：https://www.python.org/downloads/

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556266494994.png)

## Pycharm是真滴香

第二步：IDE在这里我推荐用 Pycharm  这个IDE简直就是Python开发的大宝剑好吧，用它可以将你的开发速度蹭蹭蹭地往上提，而且调试以及性能查看的功能会让你欲罢不能，逐渐上瘾的哦。
链接在此：https://www.jetbrains.com/pycharm/download/

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556266660366.png)

专业版需要激活码、或者激活服务器。不过大家可以自行百度找到，很多很多。社区版则不用激活码，但是功能会稍有不同，注意是稍有，就是基本不会影响到你的体验。
> 有条件的同学，请支持正版，可以用学校邮箱去申请专业版，也可以去开源的GitHub项目申请，方法多多的。。。

## 安装Python依赖

第三步：就是安装依赖包了
这个时候，我默认大家已经将 Pycharm 装好了，而且也可以运行Hello World 了。
在pycharm中安装库的步骤如下：
File -> Settings -> project interpreter -> + 号

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556267003527.png)

我们做PYQT5的环境搭建，需要安装这几个包：
- PyQt5-sip
- PyQt5
- PyInstaller
这几个包稍微有点大，请按照的时候保持耐心，或者使用国内镜像加速。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556267249219.png)

## 设置QT工具

第四步：添加外部设计工具
重点来了，敲黑板，哈哈哈
到这步的时候，我也默认你已经将上面那几个包给正确安装好了哈。
这个时候，我们在pycharm中添加外部设置工具。
步骤如下：
File -> Settings -> Tools -> External Tools -> + 号

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556267503467.png)

在这里我们点击 + 后，会弹出一个配置页面。设置好对应的工具名字，可以起的狂拽酷炫一点点。接着点击program那个选项的文件夹，去找到我们的designer.exe工具。这个工具一般在Python安装目录里面的那个Script目录里。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556267907261.png)

然后在working directory 里面填 \$FileDir\$，也可以点insert macros 去找到

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556268075145.png)


![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556267778683.png)

## 设置ui文件转py的工具

第五步：设置ui转py文件工具
到这步就要设置转换工具了，步骤如上，不过在 program 和 arguments 里面要改一下。
分别填入一下内容：
program ：自己Python的位置，如E:\Python36\python36.exe
arguments：-m PyQt5.uic.pyuic  \$FileName$ -o \$FileNameWithoutExtension$.py

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556268320573.png)

## 设置资源qrc文件转py的工具

第六步，设置资源转py文件工具
这个和前面一样，要改一点东西而已，也是改改 program 和 arguments的参数就可以了。
program ：自己pyrcc5.exe的位置，如E:\Python36\Scripts\pyrcc5.exe
arguments：\$FileName$ -o \$FileNameWithoutExtension$_rc.py

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556268397588.png)

这个时候，其实算是搭建好开发环境了。那为啥我还要给加上这第七步呢？在网上，有很多人的配置中，其实都没有第六步，我这里是加进去，更加完善内容的。为后面开发做好铺垫。

OK，现在我们打开设计工具，tools 》外部工具》刚才自己设置的名字

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556268586131.png)

打开后，界面就是这样子啦，点击Create，即可生成一个新的ui文件。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556268665318.png)
拖动控件，修改控件的内容，然后 Ctrl + S 保存

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556268805564.png)

记得改名，方便自己使用。

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556268969100.png)

回到pycharm里面，点击生成的ui文件，右键一下，如下

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556269086713.png)

他就会自动生成你ui文件对应的py文件了
但是，这个时候他是使用不了了，运行的时候，什么都没显示

当你有qrc文件使用到了资源文件时，使用刚才的配置rcc的那个来编译即可，我这里是叫PyQSS，这名字有点蠢~，没事见惯不怪~

## 引用自动生成的界面代码，运行代码

到第七步了，我们需要再新建一个文件，去引用我们生成的py界面文件

新建一个类，MainWindow 继承自Ui_MainWindow
然后在 \__init__ 中去调用父类的MainWindow
接着就 self.setupUi(self)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556269749447.png)

![](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-26/界面开发之PYQT5开发环境搭建/1556269582079.png)

这样子，你的第一个 Hello World 界面程序便跑出来啦，美滋滋

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>