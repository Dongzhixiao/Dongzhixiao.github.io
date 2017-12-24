---
layout:     post
title:      "回家"
subtitle:   "第一份工作的休息日"
date:       2016-10-01
author:     "WangXiaoDong"
header-img: "img/20161001.jpg"
tags:
    - 日记
    - 设计模式
    - Qt
---



### 时间:2016年10月1日 天气:小雨:umbrella:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天，回到了洛阳，因此天气是代表洛阳的天气！终于回家了！终于回家了！！终于回家了！！！
    重要的事情说三遍。实在是太激动了。今天一早，妈妈就出门锻炼身体了，我一直睡到很晚才起床。
    感觉在家睡得才舒服。今天天气有点冷，因此姥姥一直让我加衣服，但是姥姥这边没有我的长裤，因此姥姥今天一天都让我赶快回家拿长裤，一直说了一天！
    最后下午等到下完雨了，我赶快和妈妈爸爸回家把裤子拿了过来，这样姥姥终于不说我啦。
</pre>

### 设计模式(二十四)————职责链模式

职责连模式：使多个对象都有机会处理请求，从而避免请求的发送者和接收者之间的耦合关系

将这个对象连成一条链，并沿着这条链传递该请求，直到有一个对象处理它为止

就拿雇员向领导提出请假和加薪的要求，不同的领导的权限不一样来向上级请示为例子：

```C++
#ifndef MANAGER_H
#define MANAGER_H

#include <QString>
#include <QSharedPointer>
#include <QDebug>

class Request
{
public:
    QString getRequestType() const {return _requestType;}
    void setRequestType(QString requestType){_requestType = requestType;}
    QString getRequestContent() const {return _requestContent;}
    void setRequestContent(QString requestContent){_requestContent = requestContent;}
    int getNumber() const {return _number;}
    void setNumber(int number){_number = number;}
private:
    QString _requestType;
    QString _requestContent;
    int _number;
};

class Manager
{
public:
    explicit Manager(QString name)
    {
        _name = name;
    }
    void setSuperior(Manager* superior)
    {
        _superior = QSharedPointer<Manager>(superior);
    }
    virtual void RequestApplications(const Request &) = 0;
    virtual ~Manager(){}
protected:
    QString _name;
    QSharedPointer<Manager> _superior;
};

class CommonManager final : public Manager   //经理类
{
public:
    explicit CommonManager(QString name):Manager(name){}
    void RequestApplications(const Request & request) override
    //注意，函数的参数如果有const实例，则函数里面调用该实例的方法必须是const函数
    {
        if(request.getRequestType() == "请假" && request.getNumber()<=2)
        {
            qDebug()<<_name<<request.getRequestContent()<<request.getNumber()<<"被批准";
        }
        else
        {
            if(_superior!=nullptr)
                _superior->RequestApplications(request);
        }
    }
};

class Majordomo final : public Manager   //总监类
{
public:
    explicit Majordomo(QString name):Manager(name){}
    void RequestApplications(const Request & request) override
    {
        if(request.getRequestType() == "请假" && request.getNumber()<=5)
        {
            qDebug()<<_name<<request.getRequestContent()<<request.getNumber()<<"被批准";
        }
        else
        {
            if(_superior!=nullptr)
                _superior->RequestApplications(request);
        }
    }
};

class GeneralManager final : public Manager   //总经理类
{
public:
    explicit GeneralManager(QString name):Manager(name){}
    void RequestApplications(const Request & request) override
    {
        if(request.getRequestType() == "请假" && request.getNumber()<=500)
        {
            qDebug()<<_name<<request.getRequestContent()<<request.getNumber()<<"被批准";
        }
        else if(request.getRequestType() == "加薪" && request.getNumber()<=500)
        {
           qDebug()<<_name<<request.getRequestContent()<<request.getNumber()<<"被批准";
        }
        else if(request.getRequestType() == "加薪" && request.getNumber()>500)
        {
            qDebug()<<_name<<request.getRequestContent()<<request.getNumber()<<"再说吧";
        }
    }
};

#endif // MANAGER_H
```

有了管理者和各个管理者的子类，就可以实现main函数的例子了：

```C++
#include "manager.h"

int main()
{
    CommonManager* jinli = new CommonManager("金利");
    Majordomo* zongjian = new Majordomo("宗剑");
    GeneralManager* zhongjingli = new GeneralManager("钟精励");
    jinli->setSuperior(zongjian);
    zongjian->setSuperior(zhongjingli);

    Request* request = new Request();
    request->setRequestType("请假");
    request->setRequestContent("小菜请假");
    request->setNumber(1);
    jinli->RequestApplications(*request);

    Request* request2 = new Request();
    request2->setRequestType("请假");
    request2->setRequestContent("小菜请假");
    request2->setNumber(4);
    jinli->RequestApplications(*request2);

    Request* request3 = new Request();
    request3->setRequestType("加薪");
    request3->setRequestContent("小菜请求加薪");
    request3->setNumber(500);
    jinli->RequestApplications(*request3);

    Request* request4 = new Request();
    request4->setRequestType("加薪");
    request4->setRequestContent("小菜请求加薪");
    request4->setNumber(1000);
    jinli->RequestApplications(*request4);

    return 0;
}
```

由代码可以看出，职责链模式将一个管理者类抽象出来，然后扩展为三个具体的类（经理类、总监类和总经理类），这样类的灵活性就大大增加！如果我们需要扩展新的管理者，只需要增加子类即可，真的是非常灵活方便！

最后放上源码地址：https://github.com/Dongzhixiao/designMode_qt/tree/master/SalaryRaise_ResponsibilityEven_mode_24