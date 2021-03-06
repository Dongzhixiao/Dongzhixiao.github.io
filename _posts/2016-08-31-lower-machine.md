---
layout:     post
title:      "本机调试上下位机"
subtitle:   "2016/08/31 通过虚拟机模拟下位机进行调试"
date:       2016-08-31
author:     "WangXiaoDong"
header-img: "img/20160831.jpg"
tags:
    - 日记
    - Qt
    - 设计模式
    - Effect C++
---

### 时间:2016年8月31日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    这两天，项目负责人丁丁过来了，今天，他来到我们这边给我们指导，教我们如何在调试上位机程序的时候能够不需要下位机就可以运行。
	因此学习了一天相关内容，真的是感觉非常充实！
</pre>

在开始今天的学习任务之前，我先简单总结一下今天在工作中学到的东西：
- 虚拟机的安装和使用
>今天学到的第一个知识就是会使用虚拟机了，其实也很简单，就是下载一个软件，[这里](https://www.virtualbox.org/wiki/Downloads "下载地址")是下载地址。我选择的是`Windows 64位`的，然后安装。之后加载一个`系统`。这个系统是直接从别人那拷贝过来的`Ubuntu`系统，暂时还没有深入研究，以后有机会会仔细研究的。这里需要注意一点：为了让电脑能够加载虚拟机，要在电脑开机进入的`BIOS`下面将`虚拟化`标签由`disabled`改为`enabled`。即启用支持虚拟机。
- 进入本地电脑和虚拟机通讯
>在进入虚拟机并且成功启动系统后，首先进入命令行输入`ifconfig`查看当前的ip地址，然后可以进行操作了。使用虚拟机主要进行两种操作：
>>1. 将本地文件和虚拟机系统中的文件进行互操作，这时可以使用这个[地址](http://winscp.net/download/WinSCP-5.9.1-Setup.exe "WinSCP下载地址")的软件进行操作，输入之前查询到的ip地址进入，即可方便的进行文件互操作啦！
>>2. 进行命令行操作，通常情况下直接进入虚拟机打开命令行进行操作即可，但是有时候连接远程系统的时候这样是无法使用的。因此可以进入这个[地址](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html "PuTTY下载地址")的软件下载后，输入之前查询到的ip地址进入，即可方便的进入虚拟机的命令行进行操作啦！

#### 条款17：以独立语句将Newed对象放置入智能指针

假设我们有一个函数用来揭示处理程序的优先权，另一个函数用来在某动态分配所得的Widget上进行某些带有优先权的处理：

```C++
int priority();
void processWidget(std::tr1::shared_ptr<Widget> pw, int priority);
```

不要忘记使用对象管理资源的至理名言，processWidget为处理动态分配的 Widget使用了一个智能指针（在此采用tr1::shared_ptr）。现在考虑一个对processWidget 的调用：

`processWidget(new Widget, priority());`

以上调用不能通过编译。tr1::shared\_ptr构造函数需要一个原始指针，但该构造函数是个explicit构造函数，无法进行隐式转换，所以不能从一个由"new Widget"返回的原始指针隐式转换到 processWidget 所需要的 tr1::shared\_ptr。如果写成这样就可以通过编译：

```C++
processWidget(std::tr1::shared_ptr<Widget>(new Widget), priority());
```

令人惊讶的是，尽管我们在这里使用了对象管理资源，这个调用还是可能泄漏资源。

在编译器能生成一个对 processWidget 的调用之前，必须首先核算即将被传递的各个实参。在调用 processWidget之前，编译器必须为这三件事情生成代码：

- 调用 priority。

- 执行 "new Widget"。

- 调用 tr1::shared_ptr 的构造函数。

C++ 编译器允许在一个相当大的范围内决定这三件事被完成的顺序（这里与 Java 和 C# 等语言的处理方式不同，那些语言里函数参数总是按照一个特定顺序被计算）。可以确定的是"new Widget"一定在 tr1::shared\_ptr 构造函数被调用之前执行，因为这个表达式的结果要作为一个参数传递给 tr1::shared\_ptr 的构造函数，但是 priority 的调用可以排在第一第二或第三个执行。如果编译器选择第二个执行它（或许能生成更有效率的代码），我们最终得到这样一个操作顺序：

- 执行 "new Widget"。

- 调用 priority。

- 调用 tr1::shared_ptr 的构造函数。

现在如果对 priority 的调用引发一个异常将发生什么？在这种情况下，从 "new Widget" 返回的指针被丢失，因为它没有被存入我们期望能阻止资源泄漏的tr1::shared_ptr。由于一个异常可能插入“资源被创建（经由newWidget）”和“资源被转换为资源管理对象”两个时间点之间，所以调用processWidget可能会发生一次泄漏。

避免这类问题的方法很简单：使用分离语句，分别写出（1）创建Widget，（2）将它置入一个智能指针内，然后再把那个智能指针传给processWidget：

```C++
std::tr1::shared_ptr<Widget> pw(newWidget); //在单独语句内以智能指针存储newed所得对象

processWidget(pw, priority());   //这个调用动作绝不至于造成泄漏
```

这样做之所以行得通，是因为编译器对于跨越语句的各项操作没有重新排列的自由（只有在语句内才拥有那个自由度）。"new Widget" 表达式以及tr1::shared_ptr构造函数调用这两个动作，与 priority的调用在不同的语句中，所以编译器不得在它们之间任意选择执行次序。

请牢记：

- [x] ***以独立语句将newed对象存储于（置入）智能指针内。如果不这样做，一旦异常被跑出来，有可能导致难以察觉的资源泄露。***

#### 设计模式(二)————策略模式

在学习今天的设计模式之前，先要了解一个使用类的原则：

面向对象编程，并不是类越多越好，类的划分是为了封装，但分类的基础是抽象，具有相同属性和功能的对象的抽象集合集合才是类。

假如要实现一个商品价格收费的类，其中收费的策略有好几种，分别是普通金额，打折后金额，还有花费满一定金额返利。在类似实现这种情况的逻辑时，就可以使用策略模式！

策略模式：它定义了算法家族，分别封装起来，让他们之间可以相互替换，此模式让算法的变化，不好影响到使用算法的客户

首先我们可以定义收费的基类，然后根据几种收费策略来进行继承：

```C++
#ifndef CASHSUPER
#define CASHSUPER

class CashSuper{
public:
    virtual ~CashSuper(){}

    virtual double acceptMoney(double money) = 0;
};

class CashNormal final:public CashSuper
{
//protected:
    double acceptMoney(double money) override
    {
        return money;
    }
};

class CashRebate final:public CashSuper
{
    double moneyRebate = 1;
public:
    CashRebate(double rebate)
    {
        moneyRebate = rebate;
    }
//protected:
    double acceptMoney(double money) override
    {
        return money*moneyRebate;
    }
};

class CashReturn final:public CashSuper
{
    double moneyCondition = 0;
    double moneyReturn = 0;
public:
    CashReturn(double mc,double mr)
    {
        moneyCondition = mc;
        moneyReturn = mr;
    }
//protected:
    double acceptMoney(double money) override
    {
        double result = money;
        if(money >= moneyCondition)
            result = money - (money/moneyCondition) * moneyReturn;
        return result;
    }
};

#endif // CASHSUPER
```

然后可以使用一个context类，将这些继承类封装起来然后在构造函数中实现继承类的挑选，界面逻辑所需要的结果也封装在这个context类中。

```C++
#ifndef CASHCONTEXT
#define CASHCONTEXT
#include <QString>
#include "cashsuper.h"

class CashContext    //策略模式，以相同的方式调用各种算法，减少各种算法类和使用算法类的耦合，结果抽象在getResult函数里返回
//同时策略模式还简化了单元测试,因为每个算法都有自己的类，可以通过自己的接口单独测试！修改任意一个算法不会影响其他算法！！
{
    CashSuper * cs;
public:
    CashContext(QString qs)   //策略模式封装了变化,判断在策略类的函数里面完成,但是还有更好的方法：反射技术(抽象工厂模式中再学！)
    {
        if(qs == QString::fromUtf8("正常收费"))
        {
            cs = new CashNormal();
        }
        else if(qs == QString::fromUtf8("打八折"))
        {
            cs = new CashRebate(0.8);
        }
        else if(qs == QString::fromUtf8("满300返100"))
        {
            cs = new CashReturn(300,100);
        }
    }
    double getResult(double money)
    {
        return cs->acceptMoney(money);
    }
};

#endif // CASHCONTEXT
```

这样，我们再简单做一个界面就看测试这个程序啦！界面程序请参考源代码。

最后方式源码地址：https://github.com/Dongzhixiao/designMode_qt/tree/master/shopPrice_strategy_pattern