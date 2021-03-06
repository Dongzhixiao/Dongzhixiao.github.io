---
layout:     post
title:      "周日休息"
subtitle:   "2016/9/25 放松 购物"
date:       2016-09-25
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160925.jpg?raw=true"
tags:
    - 日记
    - Qt
    - 设计模式
---

### 时间:2016年9月25日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](../MyPicture.JPG "我的头像")
----------

<pre>
    今天终于可以休息了。于是早晨通过总结这一周的代码，然后看书来达到放松的目的。下午好好睡了一觉，补充上一周的疲惫心情，
    然后去超市买点东西，回到宿舍边吃边和姥姥聊天，姥姥一直期待的我回家，一想到下周要回家了，感觉实在是太棒啦！
</pre>

### 设计模式(十八)————组合模式

组合模式：将对象组合成树形结构以表示‘部分-整体’的层次结构。组合模式使得用户对单个对象和组合对象的使用具有一致性

这里使用公司总部和分公司来举例：

```C++
#ifndef COMPANY_H
#define COMPANY_H

#include <QString>
#include <QDebug>
#include <QList>

class Company    //公司类，抽象接口
{
public:
    Company(QString name):_name(name){}
    virtual void Add(Company *c) = 0;
    virtual void Remove(Company *c) = 0;
    virtual void Display(int depth) = 0;
    virtual void LineOfDuty() = 0;
    virtual ~Company() {}    //注意析构函数不能声明为纯虚函数！
protected:
    QString _name;
};

class ConcreteCompany final: public Company   //具体公司类，实现抽象接口 树枝节点
{
public:
    ConcreteCompany(QString name):Company(name){}
    ConcreteCompany(const ConcreteCompany &) = delete;   //QList里面如果使用智能指针就不用delete了
    ConcreteCompany & operator =(const ConcreteCompany &) = delete;   //QList里面如果使用智能指针就不用delete了
    void Add(Company *c) override
    {
        children.append(c);
    }
    void Remove(Company *c) override
    {
        children.removeOne(c);
    }
    void Display(int depth) override
    {
        qDebug()<<QString(depth,'-')<<_name;
        foreach (Company * c, children)
        {
            c->Display(depth+2);
        }
    }
    void LineOfDuty() override
    {
        foreach(Company *c , children)
            c->LineOfDuty();
    }
    ~ConcreteCompany() override
    {
        qDeleteAll(children.begin(),children.end());   //QList里面如果使用智能指针就不用qDeleteAll了
        children.clear();
    }

private:
    QList<Company *> children;
};

class HRDepartment final: public Company   //人力资源部类，树叶节点
{
public:
    HRDepartment(QString name):Company(name){}
    void Add(Company *) override {}
    void Remove(Company *) override {}
    void Display(int depth) override
    {
        qDebug()<<QString(depth,'-')<<_name;
    }
    void LineOfDuty() override
    {
        qDebug()<<_name<<"员工招聘培训管理";
    }
    ~HRDepartment() override {}
};

class FinanceDepartment final: public Company   //人力资源部类，树叶节点
{
public:
    FinanceDepartment(QString name):Company(name){}
    void Add(Company *) override {}
    void Remove(Company *) override {}
    void Display(int depth) override
    {
        qDebug()<<QString(depth,'-')<<_name;
    }
    void LineOfDuty() override
    {
        qDebug()<<_name<<"公司财务收支管理";
    }
    ~FinanceDepartment() override {}
};

#endif // COMPANY_H

```

有了树枝节点和树叶节点，就可以随意往里面添加内容啦，主函数可以这样举例：

```C++
#include "company.h"

int main(int argc, char* argv[])
{
    Q_UNUSED(argc); Q_UNUSED(argv);

    ConcreteCompany root("北京总公司");
    root.Add(new HRDepartment("总公司人力资源部"));
    root.Add(new FinanceDepartment("总公司财务部"));

    ConcreteCompany *comp = new ConcreteCompany("上海华东分公司");
    comp->Add(new HRDepartment("华东分公司人力资源部"));
    comp->Add(new FinanceDepartment("华东分公司财务部"));
    root.Add(comp);

    ConcreteCompany *comp1 = new ConcreteCompany("南京办事处");
    comp1->Add(new HRDepartment("南京办事处人力资源部"));
    comp1->Add(new FinanceDepartment("南京办事处公司财务部"));
    root.Add(comp1);

    ConcreteCompany *comp2 = new ConcreteCompany("杭州办事处");
    comp2->Add(new HRDepartment("杭州办事处人力资源部"));
    comp2->Add(new FinanceDepartment("杭州办事处公司财务部"));
    root.Add(comp2);

    qDebug()<<"结构图：";
    root.Display(1);
    qDebug()<<"职责：";
    root.LineOfDuty();

    return 0;
}
```

可以看出，只要组合模式的类构造好，往里面添加枝叶是非常容易的！并且使用里面的方法时可以把所有枝叶的方法都展现出来，非常好用！

最后放上源码地址：https://github.com/Dongzhixiao/designMode_qt/tree/master/CompanyAndDepartment_Composite_pattern_19