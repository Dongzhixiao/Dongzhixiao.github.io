---
layout:     post
title:      "继续学习《Effective C++》"
subtitle:   "2016/08/27 进入了资源管理模块的学习，非常激动"
date:       2016-08-27
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160827.jpg?raw=true"
tags:
    - 日记
    - Effect C++
---

### 时间:2016年8月27日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天开始，继续学习《Effective C++》！终于进入了资源管理模块，太激动了，这一块是我最想仔细研究并且掌握的内容，
	让我们开始吧！
</pre>

#### 条款13：以对象管理资源

我们都知道，当new一个东西之后，必须delete它。但是问题可能出现在new和delete之间：比如中间出现了异常，或者return之类的。一种比较好的作法是通过对象来管理：因为当对象的声明周期结束以后，会调用析构函数，而在析构函数中delete，这样的作法就靠谱多了。

假设类Investment是各种关于投资的基类，股票类Stock继承自Investment。有一个工厂函数可以返回一个具体的Investment对象：

```C++
#include<iostream>

using namespace std;

class Investment    　　　　　　　　  // 关于投资的基类
{
public:
    virtual ~Investment();
};
Investment::~Investment(){ }

class Stock : public Investment     // 股票类，继承自Investment
{

};

Investment* createInvestment()      // 工厂函数，返回一个派生类对象(此处为了简化，其实最好定义一个工厂类做这个工作)
{
    return new Stock;
}

void use()                          // 使用工厂函数返回的对象，完成工作后删除这个对象
{
    Investment* pInv = createInvestment();
    // ...
    delete pInv;
}

int main()
{
    use();                          // 正常运行

    return 0;
}
```


正常情况下，代码正常运行。现在考虑这样一个事实：如果在// ...处出现了return语句，那么是不是delete就无法执行了，如此就造成了内存泄漏。很多情况下，即使函数设计非常严密，当前不会不出现这种类似情况，但是如果后期维护人员为了调试出现的问题，自行加入了导致delete不能运行的语句块呢？也就是说delete不能执行的情况总会发生，幸好我们还有如下方法可以避免这个问题：

把需要被释放的资源放入管理对象中，这样便可以依靠C++的“析构函数自动调用机制”确保资源被释放。

注意：

此方法适合管理动态分配的heap对象。

auto_ptr是个pointer-like对象，就是所谓的智能指针，会在一个模块结束后自动调用所指对象的delete，示例如下：

```C++
void use()                          // 使用工厂函数返回的对象，完成工作后删除这个对象
{
    auto_ptr<Investment> pInv(createInvestment());  // 使用auto_ptr后不用显式调用delete了
    // ...
}
```

述代码体现了两个关键想法：

1 在获得资源后立刻放进管理对象中：即用指向具体对象的指针来初始化auto_ptr。其思想也被称为“资源获得时机便是初始化时机： Resource Acquisition Is Initialization: RAII”。

2 管理对象运行析构函数确保资源的到释放：一旦当对象离开其作用域时，对象的析构函数会被自动调用，再也不用担心内存泄漏了？当然也不是，如果析构函数出现问题呢，这就是另一个话题了。

 

我们总是在不经意间将同一对象delete不止一次，auto\_ptr也考虑到了这个问题，其规定：如通过copy构造函数或者copy assignment操作符函数copy它们，那么被copy的那个指针就会变为null，就是说确保永远只有一份auto\_ptr，就像如下：

```C++
void use()                          // 使用工厂函数返回的对象，完成工作后删除这个对象
{
    auto_ptr<Investment> pInv(createInvestment());  // 使用auto_ptr后不用显式调用delete了
    // ...
    auto_ptr<Investment> pInv2(pInv);               // 现在pInv2指向Stock对象，pInv为null
    pInv = pInv2;                                   // 现在pInv指向Stock对象，pInv2为null
}
```

但是对于某些类，比如STL容器，其要求可以同时有多个指针指向同一对象，此时，auto_ptr显得有些力不从心了，但是我们又有一个“带有计数功能的智能指针：reference-counting smart pointer: RCSP”，其可以计数共有多少指针指向同一对象，只有在计数为0时才自动释放该对象，那么use()就可以如下设计：

```C++
void use()                          // 使用工厂函数返回的对象，完成工作后删除这个对象
2 {
3     tr1::shared_ptr<Investment> pInv(createInvestment());
4     tr1::shared_ptr<Investment> pInv2(pInv);    // pInv和pInv2同时指向Stock对象
5     pInv = pInv2;                               // 同上，没有任何改变
6 }
```

注意：

auto\_ptr和tr1::shared\_ptr二者的析构动作都是执行delete而不是delete[]，就是说如果动态分配的是数组，那么使用智能指针不会释放全部资源，C++并没有针对动态分配的数组做智能化释放处理，那是因为vector和string可以完全取代数组的工作。

 

最后有一个小点，如果我们想要使用智能指针所指向的原始指针，如何做呢？

有两种方式：

1 使用显式转换：get()

Investment* pInv2 = pInv.get();

2 自己设计隐式转换函数

一般而言，显式转换虽然麻烦，但是比较安全；隐式转换虽然比较方便，但是有时会造成难以发现的错误。


请记住：

- [x] ***为防止资源泄漏，请使用RAII对象（Resource Acquisition Is Initialization，资源取得时机就是初始化时机），它们在构造函数中获得资源并在析构函数中释放资源。***

- [x] ***两个常被使用的RAII classes分别是std::tr1::shared_ptr和#include<memory>中的std::auto_ptr。***

