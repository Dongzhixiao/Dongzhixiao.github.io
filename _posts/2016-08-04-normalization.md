---
layout:     post
title:      "程序文档"
subtitle:   "2016/08/04 写程序一定要有规范的文档！"
date:       2016-08-04
author:     "WangXiaoDong"
header-img: "img/20160804.jpg"
tags:
    - 日记
    - C++
    - Effect C++
---

### 时间:2016年8月4日 天气:雷阵雨转多云:umbrella:->:cloud:
-----
#####   Author:冬之晓:dizzy_face:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天继续研究了一天的程序，有一个debug一直困扰着我，就是程序运行的时候内存
总是一直增长，我开始以为我改了一些代码所致，最后发现原来一开始丁丁给我的代码就
有问题，最后我通过git进行前后版本的对比，才发现问题出在程序中的一个线程没有暂
停引起的。最近还要研究一下文档的书写，因为我发现一个程序没有文档真的是非常难看
懂，我不希望经过我手的程序会有同样的问题，因此我决定为手头的程序制定规范和说明
文档！！
</pre>

#### 条款05：了解C++默默编写并调用哪些函数

c++的编译器是非常智能的！当你声明一个空类empty class，如果你的代码有用到这个empty class时，编译器会默默的为你编写一些基本的函数。
那么究竟编译器自己添加的函数都有哪些呢？构造函数,析构函数，一个copy构造函数和一个copy assignment操作符。举个例子来说明一下，如果你写下：

`class empty{};`

就好像你写下这样的代码：

```C++
class Empty
{
public:
	Empty(){ ... }          //default 构造函数
	~Empty(){ .... }      //析构函数

	Empty(const Empty & rhs) { ... }   //copy 构造函数
	Empty & operator=(const Empty & rhs) { ... }  //copy assignment 操作符
};
```

不过需要有一点值得注意：**只有当这些函数被调用的时候，他们才会被编译器创建出来。**如下所示：

```C++
Empty e1;   //default构造函数
Empty e2(e1);   //copy构造函数
e2 = e1;   //copy assignment操作符
```

default构造函数和析构函数主要是给编译器一个地方用来放置“藏身幕后”的代码，像是调用base classes和non-static成员变量的构造函数和析构函数。像是调用基类和非静态成员变量的构造函数和析构函数(要不然它们该在哪里被调用呢O(∩_∩)O~)。注意：编译器产生的析构函数是个non-virtual，除非这个类的基类自身声明有virtual析构函数。 

如果你不想用默认的构造函数，那么你可以自己手工创建构造函数，这是就会遮盖掉默认构造函数，不再允许调用默认的构造函数。

copy构造函数和copy assignment操作符都是属于copy操作，编译器创建的版本只是单纯地将来源对象的每一个非静态成员变量拷贝到目标对象。考虑下面的代码：

```C++
template<typename T>
class NameObject
{
public:
	NameObject(const char * name, const T& value);
	NameObject(const std::string & name, const T& value);
	~NameObject();
	....

private:
	std::string nameValue;
	T objectValue;
};

NameObject<int> no1("test string", 2);
NameObject<int> no2(no1);
```

此时no2.nameValue以no1.nameValue为实参，nameValue是string类型，所以会no2.nameValue会调用string的copy函数进行赋值

no2.objectValue是int类型，会直接拷贝no1.objectValue的每个bits进行赋值。

但是并不是所有情况编译器都可以生成operator=的方法，不满足的情况主要包括下面两种：

- 待赋值的成员变量中包含引用
- 待赋值的成员变量中包含const变量

聪明的你想必已经想到了！对了, c++并不允许“让reference改指向不同对象”。同时，对一个const成员进行赋值也是个bad idea！ 

如果你打算在一个含有reference成员的class内支持赋值操作符，你必须自己定义copy assignment操作符。

请记住：

- [x] ***编译器可以暗自为class创建default构造函数，copy构造函数，copy assignment操作符，以及析构函数。***

