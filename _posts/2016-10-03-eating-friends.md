---
layout:     post
title:      "去田田姐姐家吃饭"
subtitle:   "2016/10/3 吃饭 会友"
date:       2016-10-03
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20161003.jpg?raw=true"
tags:
    - 日记
    - 设计模式
    - Qt
---

### 时间:2016年10月3日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

<pre>
    今天，天气还是很晴朗，今天还是要去田田姐姐家吃饭，所以我就拒绝和同学邀约出门玩。
    在田田姐姐家吃的非常好。刚吃完饭，我在淘宝上买的刮胡子刀的快递突然到了，因此就急冲冲的赶回姥姥家取快递。
    下午，郭凯过来玩了，听说他刚找了一个女朋友，然后送她上火车，因此来到了市区里面。顺便来我家玩，
    刚好我们明天邀请洛阳的高中同学。晚上，我们一起玩了一会，郭凯就准备走，结果没有乘上公交车，
    我们就一起在姥姥家住了一夜。晚上，听见楼上一夜打牌的声音，想到姥姥在这里住了这么多年，
    一直生活在这样的环境中，真的非常难过，真的希望妈妈买的房子赶快装修好，让姥姥住进去！
</pre>

### 设计模式(二十六)————享元模式

享元模式：运用共享技术有效地支持大量细粒度的对象

我们以网站共享为例子：

```C++
#ifndef WEBSITE_H
#define WEBSITE_H

#include <QString>
#include <QDebug>
#include <QMap>
#include <QSharedPointer>

class User
{
public:
    User(QString name):_name(name){}
    QString GetName() {return _name;}
private:
    QString _name;
};

class WebSite   //抽象网站类
{
public:
    virtual void Use(User user){Q_UNUSED(user);}
    virtual ~WebSite() = default;
};

class ConcreteWebSite final : public WebSite   //具体网站类
{
public:
    explicit ConcreteWebSite(QString name):_name(name){}
    void Use(User user) override
    {
        qDebug()<<"网站分类："<<_name<<"用户："<<user.GetName();
    }
private:
    QString _name;
};

class WebSiteFactory
{
public:
    WebSite* GetWebSiteCategory(QString key)
    {
        if(!_flyweights.contains(key))
        {
            _flyweights.insert(key,QSharedPointer<ConcreteWebSite>(new ConcreteWebSite(key)));
        }
        return _flyweights[key].data();
    }
    int GetWebSiteCount()
    {
        return _flyweights.count();
    }
private:
    QMap<QString,QSharedPointer<ConcreteWebSite>> _flyweights;
};

#endif // WEBSITE_H
```

然后我们在main函数中模拟三个产品展示的网站和三个博客的网站。

```C++
#include "website.h"

int main()
{
    WebSiteFactory* f = new WebSiteFactory;

    WebSite* fx = f->GetWebSiteCategory("产品展示");
    fx->Use(User("小菜"));

    WebSite* fy = f->GetWebSiteCategory("产品展示");
    fy->Use(User("大鸟"));

    WebSite* fz = f->GetWebSiteCategory("产品展示");
    fz->Use(User("娇娇"));

    WebSite* fl = f->GetWebSiteCategory("博客");
    fl->Use(User("老顽童"));

    WebSite* fm = f->GetWebSiteCategory("博客");
    fm->Use(User("桃谷六仙"));

    WebSite* fn = f->GetWebSiteCategory("博客");
    fn->Use(User("南海鳄神"));

    qDebug()<<"网站分类总数："<<f->GetWebSiteCount();

    return 0;
}
```

我们发现，享元模式最后虽然使用了三个产品展示的网站和三个博客的网站，但是网站分类的总数是两个。这样就达到了共享对象的目的。

最后放上源码地址：https://github.com/Dongzhixiao/designMode_qt/tree/master/MoreProjects_Flyweight_pattern_26