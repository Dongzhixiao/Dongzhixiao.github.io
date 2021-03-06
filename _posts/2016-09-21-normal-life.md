---
layout:     post
title:      "继续工作"
subtitle:   "2016/9/21 普通的过活"
date:       2016-09-21
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160921.jpg?raw=true"
tags:
    - 日记
    - 设计模式
    - Qt
---

### 时间:2016年9月21日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](../MyPicture.JPG "我的头像")
----------


<pre>
    今天，一到单位，大家都问我昨天去哪了？哎，只能和大家都聊聊。
</pre>


### 设计模式(十五)————状态模式

状态模式：当一个对象的内在状态改变时允许改变其行为，这个对象看起来像是改变了其类

主要解决的是当控制一个对象状态转移的条件表达式过于复杂时的情况。把状态的判断逻辑转移到表示不同状态的一系列类当中，可以把复杂的判断逻辑简化！

如果将下班时间和此时的工作状态作为例子，代码如下：

```C++
#ifndef WORKSTATUS
#define WORKSTATUS
#include <QtDebug>

class State;
class Work
{
public:
    Work();
    int getTime(){return _hour;}
    void setTime(int hour){_hour = hour;}
    bool getFinished(){return _finished;}
    void setFinished(bool finished){_finished = finished;}
    void setState(State * s){_state = QSharedPointer<State>(s);}
    void writeProgram();
private:
    bool _finished = false;
    int _hour = 0;
    QSharedPointer<State> _state;
};

class State
{
public:
    virtual void writeProgram(Work w) = 0;
    virtual ~State() = default;
};
//下面逻辑倒着申明类，否则需要前置声明，还要使用指针！
class EveningState final:public State
{
public:
    void writeProgram(Work w) override
    {
        if(w.getTime() < 21)
        {
            qDebug()<<"当前时间"<<w.getTime()<<"加班哦，疲劳至极！";
        }
        else
        {
            qDebug()<<"当前时间"<<w.getTime()<<"不行了，睡着了！";
        }
    }
};
class AfternoonState final:public State
{
public:
    void writeProgram(Work w) override
    {
        if(w.getTime() < 17)
        {
            qDebug()<<"当前时间"<<w.getTime()<<"下午状态还不错，继续努力！";
        }
        else
        {
            w.setState(new EveningState());
            w.writeProgram();
        }
    }
};
class NoonState final:public State
{
public:
    void writeProgram(Work w) override
    {
        if(w.getTime() < 13)
        {
            qDebug()<<"当前时间"<<w.getTime()<<"饿了，午饭，犯困，午休！";
        }
        else
        {
            w.setState(new AfternoonState());
            w.writeProgram();
        }
    }
};
class ForenoonState final:public State
{
public:
    void writeProgram(Work w) override
    {
        if(w.getTime() < 12)
        {
            qDebug()<<"当前时间"<<w.getTime()<<"上午工作，精神百倍！";
        }
        else
        {
            w.setState(new NoonState());
            w.writeProgram();
        }
    }
};
#endif // WORKSTATUS


#include "workstatus.h"

Work::Work()
{_state = QSharedPointer<State>(new ForenoonState());}

void Work::writeProgram()
{_state->writeProgram(*this);}


#include "workstatus.h"

int main(int argc, char *argv[])
{
    Q_UNUSED(argc);Q_UNUSED(argv);
    Work *wk = new Work();
    wk->setTime(20);    //这里设置时间，代表加班工作到几点。
    wk->writeProgram();
    return 0;
}
```

通过代码可以看出，工作的类里面维护一个状态类，通过调用状态的writeProgram(*this)方法传入自己本身。然后状态类通过继承不同的状态，在不同的状态中根据工作实例中数据结构的不同，设置不同的状态子类来实现不同得业务逻辑。这样处理的好处就是当逻辑比较多变的时候，可以轻松的通过增加或者减少状态的继承类来实现不同状态的添加或减少，真的是非常灵活好用！

最后放上源码地址：https://github.com/Dongzhixiao/designMode_qt/tree/master/workOvertime_State_Pattern_16