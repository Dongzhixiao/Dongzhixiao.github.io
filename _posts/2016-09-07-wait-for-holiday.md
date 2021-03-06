---
layout:     post
title:      "和同学聊天"
subtitle:   "2016/9/7 约见十一"
date:       2016-09-07
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160907.jpg?raw=true"
tags:
    - 日记
    - Qt
    - Effect C++
    - 设计模式
---

### 时间:2016年9月7日 天气:小雨:umbrella:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天又忙了一天，在bug的海洋中遨游，晚上没事了，跟研究生的宿舍兄弟一起聊了
聊，又和腾飞聊了聊，和他相约十一洛阳见！呵呵~
</pre>

今天学到一点，没事干信号和槽不要加参数名！！！否则信号有可能识别不到！其实《C++ primer》在前言中就提到函数在头文件声明的时候完全没必要写参数的名字，只要传入参数的类型写的准确就行，只有在函数实现的时候才需要用的变量名！

今天其他的时间继续研究UDP连接，今天请教一位高手，他说UDP连接只要加入组播，就不需要断开，同一个网段上相同UDPIP和端口发送的数据都能够收到！我晕，昨天废了老大得劲编写一个不断断线重连的功能完全没用。今天改了一下。

同时我还发现QUdpSocket连接后的读取数据时可以直接读到其TCP的IP地址……我真是服了，原来可以这样用！！

#### 条款24 :若所有参数皆须类型转换，请为此采用non-member函数

首先还是下面这个class

```C++
class Rational{
public:
    Rational(int numirator = 0, 
             int denominator = 1);
    int numurator() const;
    int denominator() const; //    getter;
};
```

想要支持operator*的时候我们首先可能想到的是两种选择，一种是member operator*()，还有一种就是friend operator*().

 
首先，member function版本的operator\*带来的问题是， rational \* 2可以正常使用但是 2 \* rational 却不能。

所以首先这里要说的就是，所有的参数都需要进行隐式转换的情况下，采用non-member形式会更加的契合。

 

再者，如果不选去member function形式的operator\*,那么是采取friend方式的operator\*比较好还是说采取non friend形式的operator\*方式比较好呢

 

下面首先以非friend的方式实现了这个问题：

```C++
const Rational operator*(const Rational & lhs, const Rational & rhs)
{
     return Rational(lhs.numerator() * rhs.numerator(), lhs.denominator() * rhs.denominator());
     //通过了两个getter来处理这件事
}
```

上面这种方式不仅可以使tmpRational \* 2 以及2 \* tmpRational正常通过编译，而且相对于friend函数来说，其对类的内部情况了解的更少，那么实际上他比下面的friend版本函数更加降低了耦合：

```C++
const Rational operator*(const Rational & lhs, const Rational & rhs)
{
    return Rational(lhs.numerator * rhs.numerator, lhs.denominator * rhs.denominator);
    //这里就没有使用getter来处理这件事
}
```

有太多的假设认为，一个与某个class相关的方法如果不是一个member function那么就应该是一个friend，实际上并不是这样，上面两种的其实上面那种确实耦合性更小。

请记住：

- [x] ***如果要为某个函数的所有参数进行类型转换，那么这个函数必须是个non-member，而如果能取得non-friend non-member效果其实更佳。***



### 设计模式(九)————模板方法模式

模板方法模式：当我们要完成在某一细节层次一致的一个过程或一系列步骤，但其中个别步骤在更详细的层次上的实现可能不同时，我们通常考虑用模板方法模式处理！

其定义是：定义一个c操作中的算法的骨架，而将一些步骤延迟到子类中。模板方法使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤！

下面使用考试回答一个试卷的答案作为例子进行示例：

```C++
#ifndef TESTPAPER
#define TESTPAPER
#include <QString>
#include <QtDebug>

class TestPaper
{
public:
    TestPaper(){}
    void question1()
    {
        qDebug()<<"第一题"<<"答案"<<Answer1();
    }
    void question2()
    {
        qDebug()<<"第一题"<<"答案"<<Answer2();
    }
    virtual ~TestPaper(){}
protected:
    QString virtual Answer1()=0;
    QString virtual Answer2()=0;
};

class TestPaper1 final:public TestPaper
{
public:
    QString Answer1() override
    {
        return "A";
    }
    QString Answer2() override
    {
        return "B";
    }
};

class TestPaper2:public TestPaper
{
public:
    QString Answer1() override
    {
        return "C";
    }
    QString Answer2() override
    {
        return "D";
    }
};
#endif // TESTPAPER

#include "testpaper.h"

int main(int argc, char *argv[])
{
    TestPaper *a = new TestPaper1();
    a->question1();
    a->question2();
    delete a;
    a = new TestPaper2();
    a->question1();
    a->question2();
    return 0;
}
```

有代码可以看出，做试卷的时候每个题目都是一样的，唯一的区别是每个人的答案不同，这样就可以把题目抽象成一个基类，将题目的答案作为虚函数返回，这样子类就可以得到不同的结果而重新实现这些答案的虚函数，非常巧妙！