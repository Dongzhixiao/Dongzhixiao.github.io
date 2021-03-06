---
layout:     post
title:      "继续上火"
subtitle:   "2016/9/27 难受"
date:       2016-09-27
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160927.jpg?raw=true"
tags:
    - 日记
    - Qt
    - 设计模式
---

### 时间:2016年9月27日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com

----------

<pre>
    今天，舌头又疼了一天，一天都没有效率，吃饭也吃不下去，哎，不行，晚上得买点药。今天我终于发现了一个道理：舌头上面烂了真难受！！！
</pre>

### 设计模式(二十)————单例模式

单例模式：保证一个类仅有一个实例，并提供一个访问他的全局访问点

通常我们可以让一个全局变量使得一个对象被访问，但它不能防止你实例化多个对象一个最好的办法就是，让类自身负责保护它的唯一实例。这个类可以保证没有其他实例可以被创建，并且他可以提供一个访问该实例的方法

注意：使用单例模式，只能保证一个线程内对象不会被多次创建，而不不能保证多线程的情况。因此，需要考虑多线程的话，就要用锁

这里我们使用一个单例类作为例子：

```C++
#ifndef SINGLETON_H
#define SINGLETON_H

#include <QPointer>
#include <QMutex>

class Singleton: public QObject
{
public:
    static Singleton * GetInstance()
    {
        if(_instance == nullptr)
        {
            QMutexLocker locker(&_m);     // 这里相当于调用了_m.lock()
            _instance = new Singleton;
        }
        return _instance;
    }     //退出这个函数的时候，析构locker的时候就相当于调用了_m.unlock()
private:
    Singleton(){}
    static QPointer<Singleton> _instance;   //使用QPointer管理该指针是为了外界如果删除了该实例的内容可以保证指针_instance置为0
    static QMutex _m;    //QMutex是为了保证线程安全，即保证该类在多线程的情况下也只能创建一个实例
};

QPointer<Singleton> Singleton::_instance = nullptr;
QMutex Singleton::_m;   //
#endif // SINGLETON_H

#include <QDebug>
#include "singleton.h"

int main()
{
    Singleton* s1 = Singleton::GetInstance();
    Singleton* s2 = Singleton::GetInstance();

    if(s1 == s2)
    {
        qDebug()<<"两个对象具有相同的实例";
    }
    return 0;
}
```

通过打印结果我们可以发现，虽然我们进行了两次创建同一个类的实例，但是得到的地址是一样的，也就是说明只创建了一个类，单例模式就是使用这种方法保证一个类仅仅创建一个实例！

最后放上源码地址：https://github.com/Dongzhixiao/designMode_qt/tree/master/Singleton_Singleton_pattern_21