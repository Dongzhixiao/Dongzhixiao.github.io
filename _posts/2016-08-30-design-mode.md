---
layout:     post
title:      "增加学习任务"
subtitle:   "2016/08/30 设计模式和C++一块学习"
date:       2016-08-30
author:     "WangXiaoDong"
header-img: "img/20160830.jpg"
tags:
    - 日记
    - Qt
    - 设计模式
    - Effect C++
---

### 时间:2016年8月30日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天，决定每天除了《Effective C++》的学习，还要加上“设计模式”的学习，否则每天晚上的学习内容太少了。
</pre>

首先记录一下今天工作中遇到的问题：
- Qt控件提示
> Qt编写界面的时候，有时候需要进行提示，即鼠标悬浮在上时显示一下内容，提示客户需要进行什么样的操作。遇到这种问题，Qt有一个专门的属性`tooltip`，通过这个属性，输入的内容就可以在鼠标放在对应控件上显示提示内容啦！当然，对于`QLineEdit`这样的控件来说，如果当你没有输入的时候，或许需要有个提示，告诉客户不能为空，这时就可以用属性`placeholderText`，其字面意思是`占位文本`，就是当里面没有输入时，用灰色的字体显示的一种提示符。这两个属性就是Qt常用的提示属性，以后用到时查一下就好啦！

- Qt有QDialogButtonBox控件
>这个控件代表一个包含很多按钮的盒子。今天我在读代码的时候，发现别人在实现的时候使用了这个控件。我一开始非常不理解，感觉每一个按钮都单独的使用多好，这样就可以单独控制每一个按钮，特别是我需要实现的功能就是需要首先得到其中一个按钮的指针。最后，通过看Qt说明文档，我发现可以通过方法`button(StandardButton)`来返回对应的`QPushButton`，其中标准按钮可以参考说明文档，我使用的是`QDialogButtonBox::Ok`。在得到这个按钮后，我就可以随意操作它了。
>>后来我又思考了一下，有时候，当按钮比较多的时候，分组确实是一个很好地方法来进行管理，以后多多使用。

#### 条款16：成对使用new和delete时要采取相同形式

当使用new时（即通过new动态生成一个对象），有两件事发生：第一，内存被分配出来（通过名为operator new 的函数）；第二，针对此内存会有一个（或更多）构造函数被调用。

当使用delete时，也有两件事发生：针对此内存会有一个（或更多）析构函数被调用，然后内存才被释放（通过名为operator delete 的函数）。delete的最大问题在于：即将被删除的内存之内有究竟存有多少对象？（即将被删除的那个指针，所指的是单一对象或对象数组？）此问题的答案决定了又多少个析构函数必须被调用起来。

单一对象的内存布局不同于对象数组的内存布局：数组所用的内存包括“数组大小”记录，以便delete制定需要调用多少次析构函数。单一对象的内存则没有这笔记录。

delete[]认定指针指向一个数组，多次调用析构函数。因此切记 new和delete时要采取相同形式。

```C++
std::string* strPtr1 = new std::string;     
std::string* strPtr2 = new std::string[100];　
...
delete strPtr1; 　　　　//删除一个对象　　　　　　　　　　
delete [] strPtr2; 　　//删除一个由对象组成的数组
```

如果对strPtr1使用delete[]形式：delete会读取若干内存并解释为“数组大小”，然后多次调用析构函数。

如果对strPtr2没使用delete[]形式：可能导致99个析构函数没被调用，对象不太可能被适当删除。

也就是说，以上的两种情况都可能会导致不确定的行为哟~

对于typedef动作，当以new创建该种typedef类型对象时，应该说清楚应该以哪一种delete形式删除。

考虑下面这个例子：

```C++
typedef std::string AddressLines[4]；//每个人的地址有4行 每行是一个string<br><br>//AddressLines是个数组，如果这样使用new：
<br>std::string* pal = new AddressLines;//返回一个string* 跟new string[4]一样
```

那就必须匹配“数组形式”的delete[]：

```C++
delete pal;   　　 //行为未有定义！！！
delete [] pal; 　　//OK
```

请牢记：

- [x] ***如果在new表达式中使用[]，必须在相应的delete表达式中也使用[]。 new[]  对应  delete[] 如果在new表达式中不使用[],一定不要在相应的delete表达式中使用[]。 new 对应 delete***

 
我觉得设计模式是对于面向对象类型语言所特有的，同时也是是学习自己进行软件构建的基础。进行设计模式学习之前，首先要知道软件构建的几个要求：

- 可维护：即要改，只需更改要改的地方
- 可复用：要用的代码并非用完一次就没用了，完全可以在后来的地方重复使用
- 可扩展：如果要加入新的需求，只需添加一些内容即可
- 灵活性好：使用的代码功能可以随意变换组合

一开始，我从最基础的设计模式学起：

### 设计模式(一)————简单工厂模式

简单工厂模式是属于创建型模式，又叫做静态工厂方法（Static Factory Method）模式。简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。简单工厂模式是工厂模式家族中最简单实用的模式，可以理解为是不同工厂模式的一个特殊实现。

简单工厂模式的实质是由一个工厂类根据传入的参数，动态决定应该创建哪一个产品类（这些产品类继承自一个父类或接口）的实例。

举个例子，比如我现在要实现一个简单计算器。这时，我们可以创建一个符号基类，然后其他算术符号都可以继承这个类：

```C++
#ifndef SIGN
#define SIGN
#include <iostream>

class Sign{   //这里封装一个抽象类,因为数据比较简单，并且一开始就初始化了，因此使用默认构造函数，析构函数和拷贝构造函数
protected:
    double _A = 0;
    double _B = 0;
    double _Result = 0;
public:
    void setA(double a)
    {
        _A=a;
    }
    void setB(double b)
    {
        _B=b;
    }
    double getA(){return _A;}
    double getB(){return _B;}

    virtual double getResult() = 0;  
};
//下面用到继承
class signAdd:public Sign   //加法类，继承符号类
{
    double getResult()
    {
        _Result = _A + _B;
        return _Result;
    }
};

class signMinus:public Sign  //减法类，继承符号类
{
    double getResult()
    {
        _Result = _A - _B;
        return _Result;
    }
};

class signMultiply:public Sign
{
    double getResult()
    {
        _Result = _A * _B;
        return _Result;
    }
};

class signDivide:public Sign
{
    double getResult()
    {
        if(_B == 0){
            std::cerr<<"can't divide zero"<<std::endl;
            throw 1;
        }
        _Result = _A / _B;
        return _Result;
    }
};
#endif // SIGN
```

然后在用一个简单工厂类，用来生成对应的符号类：

```C++
#ifndef SIGNFACTOR
#define SIGNFACTOR
#include "sign.h"
#include <QString>

class Signfactory{    //简单工厂类，为了根据传入参数返回对应需要的类
    Sign * _S = NULL;
public:
    Sign * createSign(QString str){
        if(str=="+")
            _S = new signAdd;    
        else if(str=="-")
            _S = new signMinus;
        else if(str=="*")
            _S = new signMultiply;
        else if(str=="/")
            _S = new signDivide;
        else
        {
            std::cerr<<"unknow sign!";
            throw 0;
        }
        return _S;
    }
};

#endif // SIGNFACTOR
```

这样，我们就可以使用一个主函数测试这个程序啦：

```C++
#include "sign.h"
#include "signfactory.h"
#include <QCoreApplication>
#include <QDebug>

int main (int argc, char * argv[])
{
    QCoreApplication app(argc,argv);

    Signfactory sf;
    Sign *s = sf.createSign("/");   //这里用到多态
    s->setA(1);s->setB(1);
    try{
        qDebug()<<s->getResult();
    }
    catch(...)
    {}
    return app.exec();
}
```

这样一个简单的设计模式，就包含了类的三大特性：封装、继承、多态。而且这样一个设计就使得程序可以很好的扩展，比如当我需要再加入一个开根号的符号，只需要再继承一下符号类，然后加入开根号的算法，并且在工厂类里面多加入一个选择即可。这就是设计模式的高端之处！

其实在进行程序类之间的关系的分析时，还需要指定两个概念，这里先说明一下，提醒自己不要忘记：

- **合成**  也翻译成组合，是一种强的“拥有”关系，体现了严格的部分和整体的关系，部分和整体的生命周期一样。
- **聚合**  表示一种弱的“拥有”关系，体现的是A对象可以包含B对象，但B对象不是A对象的一部分。

理解上面这两个概念后，以后在寻找类之间的关系时就能够用这两个术语进行区别了。