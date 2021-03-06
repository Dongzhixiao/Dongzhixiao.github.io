---
layout:     post
title:      "吃多零食上火"
subtitle:   "2016/9/26 多喝水"
date:       2016-09-26
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160926.jpg?raw=true"
tags:
    - 日记
    - Qt
    - 设计模式
---

### 时间:2016年9月26日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](../MyPicture.JPG "我的头像")
----------

<pre>
    今天，上火了，舌头上起了泡，非常疼，感觉说话都说不成了，哎，我要多喝水！
</pre>

### 设计模式(十九)————迭代器模式

迭代器模式：提供一种方法顺序访问一个聚合对象中各个元素，而又不暴露该对象的内部表表示

迭代器模式是为遍历不同的聚集结构提供如开始、下一个、是否结束、当前哪一项等统一的接口

这里使用在公交车上让乘客买票作为例子：

```C++
#ifndef ITERATOR_H
#define ITERATOR_H

#include <QSharedPointer>
#include <QList>
class Aggregate;
class Iterator   //Iterator迭代器抽象类
{
public:
    virtual QString First() = 0;
    virtual QString Next() = 0;
    virtual bool IsDone() = 0;
    virtual QString CurrentItem() = 0;
    virtual ~Iterator(){}
};

class Aggregate   //Aggregate聚集抽象类
{
public:
    virtual Iterator* CreateIterator() = 0;
    virtual ~Aggregate(){}
};

class ConcreteAggregate;
class ConcreteIterator final: public Iterator
{
public:
    ConcreteIterator(ConcreteAggregate * a);
    QString First() override ;
    QString Next() override ;
    bool IsDone() override;
    QString CurrentItem() override;
private:
    QSharedPointer<ConcreteAggregate> _aggregate;
    int _current = 0;
};

class ConcreteAggregate final : public Aggregate
{
public:
    Iterator * CreateIterator() override {return new ConcreteIterator(this);}
    int Count() {return _items.count();}
    QString& operator[](int i) {return _items[i];}
    void set(QString s){_items.append(s);}
private:
    QList<QString> _items;
};

#endif // ITERATOR_H
```

因为头文件中使用了前向声明，因此需要源文件中实现指针的相关操作。

```C++
#include "iterator.h"

ConcreteIterator::ConcreteIterator(ConcreteAggregate *a)
{
    _aggregate = QSharedPointer<ConcreteAggregate>(a);
}

QString ConcreteIterator::First()
{
    return (*_aggregate)[0];
}

QString ConcreteIterator::Next()
{
    _current++;
    if(_current<_aggregate->Count())
        return (*_aggregate)[_current];
    return nullptr;
}

bool ConcreteIterator::IsDone()
{
    return _current >= _aggregate->Count() ? true : false;
}

QString ConcreteIterator::CurrentItem()
{
    return (*_aggregate)[_current];
}
```

最后，在main函数中添加乘客，然后迭代打印结果：

```C++
#include "iterator.h"

#include <QDebug>

int main(int argc,char* argv[])
{
    ConcreteAggregate *a = new ConcreteAggregate();

    a->set("大鸟");
    a->set("小菜");
    a->set("行李");
    a->set("老外");
    a->set("公交内部员工");
    a->set("小偷");

    Iterator *i = new ConcreteIterator(a);
    QString item = i->First();
    while(!i->IsDone())
    {
        qDebug()<< i->CurrentItem();
        i->Next();
    }

    return 0;
}
```

其实现在一般容器都已经支持迭代器来遍历各个数据了，这里自己实现简单的迭代器可以让我们更加深入的了解迭代器的运行机制，这样用起来就更加放心啦~

最后放上源码地址：https://github.com/Dongzhixiao/designMode_qt/tree/master/BuyTicket_Iterator_pattern_20
