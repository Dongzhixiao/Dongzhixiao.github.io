---
layout:     post
title:      "程序新特性"
subtitle:   "2016/9/9 查缺补漏"
date:       2016-09-09
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160909.jpg?raw=true"
tags:
    - 日记
    - Qt
    - Effect C++
    - 设计模式
---

### 时间:2016年9月9日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天，进行了一天的新特性查漏补缺，然后就下班了，想到明天后天就是周末，感
觉非常棒，又可以在床上度过啦！
</pre>

今天的工作中，我发现QUdpSocket这个类可以在连接的时候断开信号和槽函数，但是断开后就不能再次和这个槽函数连接了！
这样每当我不需要更新Udp读取的数据的时候可以断开，但是当我再次需要读取UDP数据的时候，只能再次新建一个QUdpSocket，
然后再和重新建立信号和槽才能够触发这个槽函数！这是奇怪，但是我也不知道为啥，以后有机会要向高人请教！！

同时今天还解决了一个问题，就是如何在和服务器断开连接后能够及时的响应到，说明已经和网络断开了，一般晚上使用的方法是“心跳连接”，
就是隔一段时间发送一条命令，如果服务器没有发出回应的话就说明断开了连接。我使用Qt自带的检查网络是否连接的类`QNetworkConfigurationManager`，
这个类在网络状态发生改变的时候发送一个信号`void QNetworkConfigurationManager::onlineStateChanged(bool isOnline)`，
这样就能够及时的对网络断开发生响应了，我检测了一下，大概断开后5秒左右这个信号就能发出！非常不错的效果！


#### 条款25：考虑写出一个不抛出异常的swap函数

swap是STL中的标准函数，用于交换两个对象的数值。后来swap成为异常安全编程（exception-safe programming，条款29）的脊柱，也是实现自我赋值（条款11）的一个常见机制。swap的实现如下：

```C++
template<class T>
void swap(T& a, T& b)
{
    T c = a;
    a = b;
    b = c;
}
```

只要T支持copying函数（copy构造函数和copy assignment操作符）就能允许swap函数。这个版本的实现非常简单，a复制到temp，b复制到a，最后temp复制到b。

但是对于某些类型而言，这些复制可能无一必要。例如，class中含有指针，指针指向真正的数据。这种设计常见的表现形式是所谓的“pimpl手法“（pointer to implementation，条款31）。如果以这种手法设计Widget class

```C++
class WidgetImpl{
public:
    ……
private:
    int a,b,c;              //数据很多，复制意味时间很长
    std::vector<double> b;
    ……
};

下面是pimpl实现的交换方法

class Widget{
public:
    Widget(const Widget& rhs);
    Widget& operator=(const Widget& rhs
    {
        ……          //复制Widget时，复制WidgetImpl对象              
        *pImpl=*(ths.pImpl);
        ……
    }
    ……
private:
    WidgetImpl* pImpl;//指针，含有Widget的数据
};
```

如果置换两个Widget对象值，只需要置换其pImpl指针，但STL中的swap算法不知道这一点，它不只是复制三个Widgets，还复制WidgetImpl对象，非常低效。

我们希望告诉std::swap，当Widget被置换时，只需要置换其内部的pImpl指针即可，下面是基本构想，但这个形式无法编译（不能访问private）。

```C++
namespace std{
    template<>      //这是std::swap针对T是Widget的特换版本，
    void swap<Widget>(Widget& a, Widget& b) //目前还无法编译
    {       //只需要置换指针
        swap(a.pImpl, b.pImpl); 
    }
}
```

其中template<>表示std::swap的一个全特化（total template specialization），函数名之后的<Widget>表示这一特化版本系针对T是Widget而设计的。我们被允许改变std命名空间的任何代码，但是可以为标准的template编写特化版本，使它专属于我们自己的class。

上面函数试图访问private数据，因此无法编译。我们可以将swap函数声明为friend，但这个和以往有点不同。可以令Widget的swap函数为public，然后将std::swap特化

```C++
calss Widget{
public:
    ……
    void swap(Widget& other)
    {
        using std::swap;//这个声明有必要
        swap(pImpl, other.pImpl);
    }
    ……
}；
namespace std{
    template<> //修订后的swap版本
    void swap<Widget>(Widget& a, Widget& b)
    {
        a.swap(b);  //调用其成员函数
    }
}
```

这个做法还跟STL容器保持一致，因为STL容器也提供public swap和特化的std::swap（用来条用前者）。 
刚刚假设Widget和WidgetImpl都是class，而不是class template，如果是template时：

```C++
template<typename T>
class WidgetImpl{……};
template<typename T>
class Widget{……};
```

可以在Widget内或WidgetImpl内放个swap成员函数，像上面一样。但是在特化std:swap 时会遇到麻烦

```C++
namespace std{
    template<typename T>
    void swap<Widget<T> >(Widget<T>& a,//不合法，错误
                          Widget<T>& b)
    {
        a.swap(b);
    }
}
```

看起来合理却不合法。上面是企图偏特化（partially specialize）一个function template(std::swap)，但C++只允许对class template偏特化，在function templates身上偏特化行不通，这段代码不该通过编译。 
当偏特化一个function template时，通常简单地为它添加一个重载版本

```C++
namespace std{
    template<typename T>//std::swap一个重载版本
    void swap(Widget<T>& a,//swap后面没有<……>
              Widget<T>& b)//这个也不合法
    {
        a.swap(b);
    }
}
```

一般而言，重载function template没有任何问题，但std是个特殊的命名空间，其管理规则也比较特殊。客户可以全特化std内的templates，但是不可以添加新的classes或functions到std里面。std的内容有c++标准委员会决定，标准委员会禁止我们膨胀那些已经 声明好的东西。 
正确的做法是声明一个non-member swap 让他来调用member swap，但不再将那个non-member swap声明为std::swap。把Widget相关机能都置于命名空间WidgetStuff

```C++
namespace WidgetStuff{
   ……//模板化的WidgetImpl等
   template<typename T>//内含swap函数
   class Widget{……};
   ……
   template<typename T>
   void swap(Widget<T>& a,//non-member,不属于std命名空间
             Widget<T>& b)
   {
       a.swap(b);
   }
}
```

上面的做法对于class和class template都适用，但是如果你想让你的“class专属版”swap在尽可能多的语境下被调用，你需要同时在该class所在命名空间内写一个non-member版本以及一个std::swap版本。 
现在所做的都与swap相关，换位思考一下，从客户角度来看，假设你正在写一个function template，其内需要置换两个对象的值

```C++
template<typename T>
void doSomething(T& obj1, T& obj2)
{
    ……
    swap(obj1, obj2);
    ……
}
```

这时应该调用哪个swap？是std既有的，还是某个可能存在的特化版本，再或则是可能存在一个可能存在的T专属版本且可能栖身于某个命名空间。我们希望首先调用T的专属版本，当该版本不存在的情况下调用std的一般户版本。

```C++
template<typename T>
void doSomething(T& obj1, T& obj2)
{
    using std::swap;//令std::swap在此函数内可用
    ……
    swap(obj1, obj2);//位T类型调用最佳版本swap
    ……
}
```

当编译器看到对swap调用时，便去找最合适的。C++的名称查找法则（name lookup rules）确保找到global作用域或T所在命名空间内的任何T专属的swap。如果T是Widget并在命名空间WidgetStuff内，编译器或使用“实参取决之查找规则”（argument-dependent lookup）找到WidgetStuff内的swap，如果没有专属版的swap，那么会调用std内的swap（因为使用了using std::swap)。

现在已经讨论了dufault swap、member swap、non-member swaps、std::swap特化版本、以及对swap的调用，下面做个总结。 
首先，如果如果swap的缺省实现对我们的class或class template效率可以接受，那么无需做任何事。 
其次，如果swap缺省实现版 的效率不足（例如，你的class或template使用了某种pimple手法），试着做以下事情： 
1、提供一个public swap成员函数，让它高效置换两个对象值。这个函数不应该抛出异常。 
2、在你的class或template所在命名空间提供一个non-member swap，并令它调用上述swap成员函数。 
3、如果你编写的是class（不是class template），为你的class 特化std::swap，并令它调用你的swap成员函数。 
如果调用swap，那么要使用using声明式，确保让std::swap在你的函数内可见。 
成员版的swap函数决不能抛出异常，因为swap的一个最好应用是帮助class或class template提供强烈的异常安全性（exception-safety）保障。条款29对此提供细节，但此技术基于一个假设：成员版swap绝不抛出异常。这个约束只施行在成员版，不用于非成员版。因为std::swap是以copy函数为基础，而copy函数允许抛出异常。 
当我们编写自定版的swap时，不是仅仅提供高效的置换对象值的方法，还要不抛出异常。这两个特性总是连在一起，因为高效的swap几乎总是基于对内置类型（例如pimple手法的底层指针），而内置类型上操作不允许抛出异常。 


请记住：


- [x] ***如果std::swap不高效时，提供一个swap成员函数，并且确定这个函数不抛出异常。 ***
- [x] ***如果提供一个member-swap,也应该提供一个non-member swap来调用前者。对于class（非class template），要特化std::swap。 ***
- [x] ***调用swap时，针对std::swap使用using形式，然后调用swap并且不带任何命名空间资格修饰。 ***
- [x] ***为“用户定义类型”进行std template全特化时，不要试图在std内加入某些对std而言是全新的东西***

### 设计模式(十一)————外观模式

外观模式：为子系统中的一组接口提供一个一致的界面，次模式定义了一个高层接口，这个接口使得这一子系统更加容易使用！

其完美的体现了依赖倒转原则和迪米特法则的思想！

比如说举一个买卖股票的例子。

```C++
#define use_sharedPtr

#ifndef FUND
#define FUND
#include <QString>
#include <QtDebug>

class Stock1
{
public:
    void buy(){qDebug()<<"买入股票1";}
    void sell(){qDebug()<<"卖出股票1";}
};
class Stock2
{
public:
    void buy(){qDebug()<<"买入股票2";}
    void sell(){qDebug()<<"卖出股票2";}
};
class Stock3
{
public:
    void buy(){qDebug()<<"买入股票3";}
    void sell(){qDebug()<<"卖出股票3";}
};
//一般需要自己写析构函数的类，都需要实现复制构造函数和赋值运算符重载，以便能够完成深拷贝
class Fund
{
public:
    Fund()
    {
#ifdef use_sharedPtr
        _stock1 = QSharedPointer<Stock1>(new Stock1());
        _stock2 = QSharedPointer<Stock2>(new Stock2());
        _stock3 = QSharedPointer<Stock3>(new Stock3());
#else
        _stock1 = new Stock1();
        _stock2 = new Stock2();
        _stock3 = new Stock3();
#endif
    }
#ifndef use_sharedPtr
    Fund(const Fund & f)   //一般类里面有指针，且析构函数销毁了，则需要完成复制构造函数来深拷贝！！
    {
        _stock1 = new Stock1(*f._stock1);
        _stock2 = new Stock2(*f._stock2);
        _stock3 = new Stock3(*f._stock3);
    }
    Fund& operator=(const Fund& a)
    {
        if(this == &a)    //判断相等的情况！！
            return *this;
        if(_stock1 != NULL)
            delete _stock1;
        if(_stock2 != NULL)
            delete _stock2;
        if(_stock3 != NULL)
            delete _stock3;
        _stock1 = new Stock1(*a._stock1);
        _stock2 = new Stock2(*a._stock2);
        _stock3 = new Stock3(*a._stock3);
        return *this;
    }
    ~Fund()
    {
        delete _stock1;
        _stock1 = NULL;
        delete _stock2;
        _stock2 = NULL;
        delete _stock3;
        _stock3 = NULL;
    }
#endif
    void buy()
    {
        _stock1->buy();
        _stock2->buy();
        _stock3->buy();
    }
    void sell()
    {
        _stock1->sell();
        _stock2->sell();
        _stock3->sell();
    }
private:
#ifdef use_sharedPtr
    QSharedPointer<Stock1> _stock1;
    QSharedPointer<Stock2> _stock2;
    QSharedPointer<Stock3> _stock3;
#else
    Stock1 * _stock1;
    Stock2 * _stock2;
    Stock3 * _stock3;
#endif
};

#endif // FUND

#include "fund.h"

int main(int argc, char *argv[])
{
    Fund *fd = new Fund();
    Fund cd = *fd;   //调用复制构造函数
//    Fund cd;
//    cd = *fd;
    delete fd;
    fd=NULL;
    cd.buy();
    cd.sell();
    return 0;
}
```

这里买卖股票的行为可以使用一个公共的类来管理所有的股票，这样就可以将买卖股票提供一个简单的接口，降低了耦合度，又化简了操作，真是一举两得啊！