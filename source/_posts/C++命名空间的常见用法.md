---
title: C++命名空间的常见用法
tags:
  - C++
  - 命名空间
categories: C++
keywords:
  - C++
  - 命名空间
description: C++命名空间的常见用法
cover: >-
  https://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2020-3-28/BogImages/1585372465268.png
abbrlink: d1d36565
date: 2019-04-16 00:00:00
---


## 前言

相信很多同学都用过using namespace std; 这句话吧。在刚学习C++的时候，教科书上告诉我们，这个叫命名空间，用上这句话后，就不用像下面这样输出
``` c++
std::cout << "Hello MARX" <<endl;
```

 可以直接用cout<<就可以输出

想必大家都知道using namespace std;这句话的含义吧。

用上这句话后，这样命名空间std内定义的所有标识符都有效，而std又是C++的一个标准库。所以，声明好之后，就可以快速使用到里面的东西了，如 函数、类、变量 等等。

其实命名空间是98年之后才引入的，是对作用域的一种特殊的抽象，它包含了处于该作用域内的标识符，且本身也用一个标识符来表示。
就好比如，MARX是A公司的一个攻城狮，身份标识符为2333，而CBR也是B公司的攻城狮，身份标识符竟然也是2333。这个时候，MARX在A公司里面工作的好好地，CBR也在B公司里面认真地工作。他们在不同的公司里面有着相同的干着不同的活，却互不影响。

## 干说不练假把戏

命名空间的声明如下：
``` c++
namespace marx {
  int cbr;
}
```

我们可以声明多个命名空间，然后还可以嵌套使用。就是类似这样子：

``` c++
namespace marx {
	void first() {
		cout << \__FUNCTION__ << " function first"  << endl;
	}
	namespace cbr {
		void info() {
			cout << \__FUNCTION__ << "嵌套的命名空间" << endl;
		}
	}
}
```

嵌套的命名空间该怎么用呢？
比如我上面那个，marx，在使用到嵌套的 cbr 时。
可以这样声明： using namespace marx::cbr;
然后就可以直接使用到里面的info函数了。
也可以只声明marx，using namespace marx;，这样子，使用到info函数的时候，就要用cbr::info()来使用了。

那，有很多同学看到这些特征的时候，可能就会感觉，这不就是类吗？

不，这其实还是有区别的，命名空间是可以包含类的。
还有，命名空间可以重新打开然后直接在里面添加内容的，但是类就不行。
``` c++
//第二个命名空间
namespace marx_two {
	void first_cbr() {
		cout << \__FUNCTION__ << " function first" << endl;
	}
	void second(const int num) {
		cout <<\__FUNCTION__ << " 第二个函数，将数字相加后结果如下：" << num + num << endl;
	}
}

namespace marx_two {
	void third() {
		cout <<\__FUNCTION__ << " function third,直接添加进来的，类class做不到" << endl;
	}
}
```
对于命名空间，进行上面那样的操作是合法的，但是对于类来说，就是非法的了
```c++
class A {
public:
	void f1() {
		cout <<\__FUNCTION__ << endl;
	}
};
class A {
public:
	void f2() {
		cout <<\__FUNCTION__ << endl;
	}
};

```
会有如下报错~~

![](http://marxcbr.oss-cn-shenzhen.aliyuncs.com/MARXCBR/2019-4-16/C%2B%2B_%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4/1555409668538.png)

在命名空里面使用到 inline 内联命名空间的时候，内联名字空间的名称在名称查找时被特别对待，引用其中的名称时，被内联的名字空间名称可以省略。也即，内联名字空间内的标识符被提升到包含着内联的名字空间的那个父级的名字空间中。内联名字空间可以在修改名字空间名称的同时避免在二进制文件中生成的符号改变，因此不同内联名字空间的名称可以用于标识接口兼容的不同版本，有助于保持二进制兼容性。

``` c++
//第二个命名空间
namespace marx_two {
	void first_cbr() {
		cout << __FUNCTION__ << " function first" << endl;
	}
	void second(const int num) {
		cout << __FUNCTION__ << " 第二个函数，将数字相加后结果如下：" << num + num << endl;
	}
	inline namespace myin
	{
		void special() {
			cout << "使用inline内联命名空间时，命名空间可以被省略"<<endl;
		}
	}
}
```
声明了 using namespace marx_two;之后，再special();就可以直接使用了。
那差不多就这样了


<center>要是感觉不错的话，就留个言评论一下下呀，欢迎各位大哥指正</center>