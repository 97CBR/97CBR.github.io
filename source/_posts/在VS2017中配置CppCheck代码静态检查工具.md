---
title: 在VS2017中配置CppCheck代码静态检查工具
tags:
  - C++
  - 安全
  - 静态检查
  - Visual Studio
categories:
    - [Visual Studio]
    - [C++]
keywords:
  - 静态检查
  - Visual Studio
  - C++
description: 在VS2017中配置CppCheck代码静态检查工具去检查C++的代码
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-3/BogImages/1585899065456.png
abbrlink: db7259e3
date: 2020-04-03 14:45:42
---

## 前言

最近代码写的稍微有点多，有写新功能，也有做旧代码的重构
回顾之前的代码，总是感觉当年写的像一坨翔\~
这写的都是啥呀，哇\~好丑的格式、好烂的代码……
运行起来的时候，哇\~又崩了\~哇又引发异常了\~怎么又访问冲突了呀

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-3/BogImages/1585896957485.png)

所以，我们不能单靠VS自带的检查工具，虽然它也能检查出一些问题。但是我显然需要更强大的检查扫描工具
鉴于之前在华为实习时将代码丢到CI/CD中被**PCLint**吊锤，无限警告报错的经历
我第一时间就想到了配置PCLint。然后查了一下，卧槽，要收费？没钱没钱~
接着就开源真香系列，CppCheck 启动！

## 简单介绍一下CppCheck

>Cppcheck是一种用于C和C ++ 编程语言的静态代码分析工具。 它是一个多功能工具，可以检查非标准代码。 [2] 创始人和首席开发人员是DanielMarjamäki。
Cppcheck是GNU通用公共许可证下的免费软件 。
>- 来自维基百科解释 [Cppcheck](https://zh.wikipedia.org/wiki/Cppcheck)

## 开始安装搭建

### 到官网下载msi

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-3/BogImages/1585897700228.png)

我们[点击链接](https://github.com/danmar/cppcheck/releases)下载文件

### 双击打开msi安装包，按照默认下一步即可
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585817194646.png)

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585817218184.png)


### 安装VS的插件

在扩展和更新里面搜索安装

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-3/BogImages/1585897820994.png)

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585817307359.png)

这里有意思的是，你在里面点下载之后，他会给你“虚晃一枪”给他把github仓库的链接给你打开了。然后自己点击下载就可

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585817290927.png)

双击安装，他会提示你关掉VS

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585817329382.png)

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585817381605.png)

## 测试使用

### VS直接编译

这里我测试两个项目

之前文章用到的dll的代码编译结果
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-3/BogImages/1585898026077.png)

之前文章提到的毕业设计的代码编译结果
![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585817563127.png)

可以看到VS一个问题都没给我找到

### 在VS里面用CppCheck分析扫描一下

毕业设计的代码扫描结果，上来就给我警告内存泄露、未使用的变量等等提示

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585817639691.png)

根据提示，修正之后，再扫描一次

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-2/BogImages/1585817799527.png)

扫描一下DLL的那个项目的代码

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-3/BogImages/1585898308038.png)

当然也给我挑出了那些越界的，未使用的变量和函数等问题

举个栗子，这个代码显然越界了，对应了上图230行的警告。

``` c++
        char buffer[3];

        ReadProcessMemory(pubgHandle, (LPVOID)GetModuleBaseAddress(g_pid, processName), buffer, 2, NULL);
        buffer[3] = '\0';
```

### 直接使用GUI客户端

可以在客户端里面打开项目进行扫描分析

![MarxCBR的小屋](https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-4-3/BogImages/1585898676469.png)

综合来说，好评哦，搭建配置这么简单，就可以提高自己的代码规范，血赚有没有！

<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>