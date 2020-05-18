---
title: VS2017 调试DLL的操作办法
tags:
  - 调试
  - Visual Studio
categories: Visual Studio
keywords:
  - 调试
  - 调试DLL
description: VS2017 调试DLL的操作办法
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-1/BogImages/1585724538247.png
abbrlink: cbfbcb33
date: 2020-04-01 14:00:33
---

## 前言

最近在搞Hook这东西玩，但是，感觉这玩意玩起来有点玄学的意味呀
所以，就想着用调试去揭开他的神秘面纱

## 简单介绍一下DLL

以下是来自百度百科的解释
>动态链接库英文为DLL，是Dynamic Link Library的缩写。DLL是一个包含可由多个程序，同时使用的代码和数据的库。在Windows中，这种文件被称为应用程序拓展。例如，在 Windows 操作系统中，Comdlg32.dll 执行与对话框有关的常见函数。因此，每个程序都可以使用该 DLL 中包含的功能来实现“打开”对话框。这有助于避免代码重用和促进内存的有效使用。 通过使用 DLL，程序可以实现模块化，由相对独立的组件组成。例如，一个计账程序可以按模块来销售。可以在运行时将各个模块加载到主程序中（如果安装了相应模块）。因为模块是彼此独立的，所以程序的加载速度更快，而且模块只在相应的功能被请求时才加载。

## 在VS2017中对DLL工程进行设置

### 进入项目设置

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-1/BogImages/1585721941987.png)

这里可以分成两种调试办法，一种是非附加调试，另一种是附加到其他进程调试

区别是什么呢，下面分成两种说明

### 非附加调试

非附加调试，一般适用于：
本地开发了一个软件，其中某些地方调用了 LoadLibrary 函数去加载我的DLL
例如：

``` c++
    HINSTANCE hDLL = ::LoadLibrary(L"HOOK.dll");
    if (hDLL) {
        // do your work
    }
```

这个时候，我们要怎么设置项目配置呢
按照以下步骤

- 调试
- 本地Windows调试器
- 命令 即将调用到这个dll的exe
- 工作目录 .

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-1/BogImages/1585722040334.png)

### 附加到其他进程调试

附加到其他进程调试，一般适用于：
本地开发了一个DLL，某个程序会明确的调用到我的DLL，或者，我们通过某些手段使得 目标程序 调用到了我的DLL
例如： 我之前发的那篇文章，利用 detours Hook别人的函数，就需要注入dll。

这个时候，我们要怎么设置项目配置呢
按照以下步骤

- 调试
- 本地Windows调试器
- 命令 即将调用到这个dll的exe
- 附加 是

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-1/BogImages/1585721992676.png)

## 测试一下调试效果

我们启动会加载我们将要调试的DLL的进程

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-1/BogImages/1585722821253.png)

## 记录一下小坑

在目标EXE未加载我们的DLL之前进行调试，是无法命中我们的断点的

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-1/BogImages/1585722365251.png)

在目标进程加载我们的DLL之后，我们的调试则可以命中断点

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-1/BogImages/1585722506415.png)

<center> 要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正 </center>