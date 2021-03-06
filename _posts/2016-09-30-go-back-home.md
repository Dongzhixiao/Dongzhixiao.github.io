---
layout:     post
title:      "十一国庆回家！"
subtitle:   "2016/9/30 回家"
date:       2016-09-30
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160930.jpg?raw=true"
tags:
    - 日记
    - Qt
    - 设计模式
---

### 时间:2016年9月30日 天气:晴:sunny:
-----
#####   Author:冬之晓:laughing:
#####   Email: 347916416@qq.com
----------

<pre>
    今天就要坐上回家的火车啦，真的非常兴奋！早晨很早就起床了，然后收拾好东西，就来的了高铁站。发现领票的人排队好长！
    不过我一到，边上几个自动取票机就开了，然后我就直接过去取了票，运气真好！在火车上，没事干，就打开《C++ primer》看了一会。
    这时，田田姐姐给我打电话啦，原来她也快到家了。果然还是自己开车比较方便呀！下午，听说我要回家，姥姥和妈妈都过过来接我了！
    看到妈妈和姥姥，真的很高兴！
</pre>

### 设计模式(二十三)————命令模式

命令模式：将一个请求封装为一个对象，从而使你可用不同的请求对客户进行参数化；对请求排队或记录请求日志，以及支持可撤销的操作

就拿在烧烤店吃小吃作为例子，把服务员作为传递指令的对象：

```C++
#ifndef COMMAND_H
#define COMMAND_H

#include <QDebug>

class Barbecuer
{
public:
    void BakeMutton()
    {
        qDebug()<<"拷羊肉串";
    }
    void BakeChickenWing()
    {
        qDebug()<<"拷鸡翅";
    }
};

class Command   //抽象命令类
{
public:
    explicit Command(Barbecuer* receiver)
    {
        _receiver = QSharedPointer<Barbecuer>(receiver);
    }
    virtual void ExcuteCommand() = 0;
    virtual ~Command(){}
protected:
    QSharedPointer<Barbecuer> _receiver;
};

class BakeMuttonCommand final : public Command
{
public:
    explicit BakeMuttonCommand(Barbecuer* receiver):Command(receiver){}
    void ExcuteCommand() override {_receiver->BakeMutton();}
};

class BakeChickenWingCommand final : public Command
{
public:
    explicit BakeChickenWingCommand(Barbecuer* receiver):Command(receiver){}
    void ExcuteCommand() override {_receiver->BakeChickenWing();}
};

class Waiter
{
public:
    void SetOrder(Command * command)
    {
        _command = QSharedPointer<Command>(command);
    }
    void Notify()
    {
        _command->ExcuteCommand();
    }
private:
    QSharedPointer<Command> _command;
};

#endif // COMMAND_H
```

有了烤串的大厨，命令和服务员，就可以在mian函数中组织这些逻辑了：

```C++
#include "command.h"

int main()
{
    //开店前的准备
    Barbecuer* boy = new Barbecuer;
    Command* bakeMuttonCommand1 = new BakeMuttonCommand(boy);
    Command* bakeMuttonCommand2 = new BakeMuttonCommand(boy);
    Command* bakeChickenWingCommand1 = new BakeChickenWingCommand(boy);
    Waiter* girl = new Waiter;

    //开门营业
    girl->SetOrder(bakeMuttonCommand1);
    girl->Notify();
    girl->SetOrder(bakeMuttonCommand2);
    girl->Notify();
    girl->SetOrder(bakeChickenWingCommand1);
    girl->Notify();

    return 0;
}
```

根据上面的代码可以看出通过把命令封装成为对象，然后可以将不同的命令参数化，这样就可以方便的对这些请求进行排队，记录或者撤销了！

最后放上源码地址：https://github.com/Dongzhixiao/designMode_qt/tree/master/Barbecue_Command_pattern_23