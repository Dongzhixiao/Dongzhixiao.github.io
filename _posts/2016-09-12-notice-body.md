---
layout:     post
title:      "头晕的一天"
subtitle:   "2016/9/12 注意身体"
date:       2016-09-12
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160912.jpg?raw=true"
tags:
    - 日记
    - Qt
    - Effect C++
    - 设计模式
---

### 时间:2016年9月12日 天气:小雨转晴:umbrella:→:sunny:

-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天，一天都比较头晕，不知道为啥，估计这几天电脑看的时间太长了，今天晚上必须早点睡了
。以后也要适当注意身体，做程序真的是很费体力的。最近快到中秋节了，单位发了一张购
物券，中秋节的时候准备去买点东西吃，呵呵。
</pre>

今天在工作中，遇到一个需求，就是界面的一个对话框中需要填入需求的字符串，因为如果填的字符串是随意的内容的话，下载到下位机上可能会使得下位机崩溃。因此我研究了一下Qt中的`QLineEdit`控件中使用正则表达式。首先查了一些文档，并且对此进行设置，发现只要新建一个`QLineEdit`后调用其中的`QLineEdit::setValidator(const QValidator * v)`即可。其中的`QValidator`派生了四个类`QDoubleValidator, QIntValidator, QRegExpValidator, 和 QRegularExpressionValidator`。根据多态直接传入其子类`QRegExpValidator`或者`QRegularExpressionValidator`即可进行正则表达式的输入。其中`QRegExpValidator`构造时传入的`QRegExp`类来进行正则表达式的构造。Qt5出来后推荐使用`QRegularExpressionValidator`传入的参数`QRegularExpression`进行正则表达式的构造。

当然构造的方式我就不详细说了，网上一搜有好多。这里举一个使用正则表达式限制`QLineEdit`的测试例子：

```C++
#ifndef DIALOG_H
#define DIALOG_H

#include <QDialog>

namespace Ui {
class Dialog;
}

class Dialog : public QDialog
{
    Q_OBJECT

public:
    explicit Dialog(QWidget *parent = 0);
    ~Dialog();

public Q_SLOTS:
    void accept() override;

private:
    Ui::Dialog *ui;
};

#endif // DIALOG_H

#include "dialog.h"
#include "ui_dialog.h"

#include <QDebug>
#include <QMessageBox>

Dialog::Dialog(QWidget *parent) :
    QDialog(parent),
    ui(new Ui::Dialog)
{
    ui->setupUi(this);
    ui->lineEdit_1->setValidator(new QRegExpValidator(QRegExp(tr("[0-9]+")),this));  //设置只能输入数字
    ui->lineEdit_2->setValidator(new QRegExpValidator(QRegExp(tr("[-]?[0-9]+,[0-9]+")),this));  //设置
}

Dialog::~Dialog()
{
    delete ui;
}

void Dialog::accept()
{
    if(!QRegExp(tr("[0-9]+")).exactMatch(ui->lineEdit_1->text()))
    {
        QMessageBox temMB(QMessageBox::Warning,
                          tr("警告"),
                          tr("第一行输入错误，默认输入是1\n请问是否设置为默认值？"),
                          QMessageBox::Yes | QMessageBox::No);
        temMB.setButtonText(QMessageBox::Yes,tr("是"));
        temMB.setButtonText(QMessageBox::No,tr("否"));
        if(temMB.exec() == QMessageBox::Yes)
            ui->lineEdit_1->setText(tr("1"));
        return;
    }
    if(!QRegExp(tr("[-]?[0-9]+,[0-9]+")).exactMatch(ui->lineEdit_2->text()))
    {
        QMessageBox temMB(QMessageBox::Warning,
                          tr("警告"),
                          tr("第二行输入错误，默认输入是1,1\n请问是否设置为默认值？"),
                          QMessageBox::Yes | QMessageBox::No);
        temMB.setButtonText(QMessageBox::Yes,tr("是"));
        temMB.setButtonText(QMessageBox::No,tr("否"));
        if(temMB.exec() == QMessageBox::Yes)
            ui->lineEdit_2->setText(tr("1,1"));
        return;
    }
    qDebug()<<ui->lineEdit_1->text()<<'\n'<<ui->lineEdit_2->text();
    QDialog::accept();
}
```

注意，我发现虽然设置了正真表达式后，`QLineEdit`只能保证输入的时候可以按照其规则输入，但是不能保证客户使用的时候输入完全，比如需要输入坐标(23,34)，客户可能没有输入逗号，还是可以按确定按钮的，为了让其完全有保证，必须在按下接受按钮的时候再次检验，这是可以重写`QDialog`的`accept()`函数，并在里面再次使用正则表达式`QRegExp`类的`exactMatch`进行匹配来达到目的。

#### 条款28：避免返回handles指向对象内部成分

首先是一个矩形的例子：

```C++
class Point{
public:
    point(int x, int y);
    ...
    void setX(int newVal);
    void setY(int newVal);
    ...
};
struct RectData{
    Point ulhc; //左上角
    Point lrhc; //右下角
};
class Rectangle{
    ...
private:
    shared_ptr<RectData> pData;
};
```

那么如果出现下面这种成员函数的话：

```C++
class Rectangle{
public:
    ...
    Point & upperLeft() const {return pData->ulhc; }
    Point & lowerRight() const{return pData->lrhc; }
    ...
}
```

这样只是能通过编译，但是设计确是错误的，在成员函数被声明为const的情况下返回了一个内部成员的引用，这样使得ulhc 以及 lrhc对象都可以被更改。但是二者都是private的，实际上二者都是不应该被改变的。
这正是由于返回的是引用，同样的返回指针，迭代其这种叫做handle的对象都是会造成内部状态暴露在外面。
这些问题实际上在每个方法的返回的handle上加上const就会得到解决。

```C++
class Rectangle{
public:
    ...
    const Point & upperLeft() const {return pData->ulhc; }
    const Point & lowerRight() const{return pData->lrhc; }
    ...
}
```

但是这样做仍然可能会导致造成虚调handle的现象发生：

```C++
class GUIobject{...};
const Rectangle
    boundingBox(const GUIobject & obj);
//那么，当用户这样去使用他的时候
GUIobject * pgo;
...
const Point * pUpperLeft = 
    &(boundingBox(*pgo).pUpperLeft());
```

这样实际上将Rectancle中的Point对象传递出去了，这样当临时对象析构的时候，这个指针就会编程一个空悬的指针（感觉通过智能指针就能解决啊），这样也带来了风险。
 
请记住：

- [x] ***避免返回Handle指向内部对象，这样可以增加封装性，帮助const函数的行为像是一个const，并将空悬Handle的可能性降到最低。***


### 设计模式(十四)————抽象工厂模式

抽象工厂模式:提供一个创建一系列相关或相互依赖对象的接口，而无需指定它们具体的类！

这个例子也可以用简单工厂模式+反射+读取配置文件来完成，这样更加简洁！！！

下面通过一个模拟访问数据库的例子来进行说明：

```C++
#ifndef USER
#define USER
#include <QString>
#include <QtDebug>

class User
{
public:
    User() = default;
    int getID(){return _id;}
    void setID(int id){_id = id;}
    QString getName(){return _name;}
    void setName(QString name){_name = name;}
private:
    int _id;
    QString _name;
};

class IUser
{
public:
    virtual void Insert(User user) = 0;
    virtual User* getUser(int id) = 0;
};
class SqlServerUser final: public IUser
{
public:
    void Insert(User user) override
    {
        Q_UNUSED(user);
        qDebug() <<"在SQL Server中给User表增加一条记录";
    }
    User* getUser(int id) override
    {
        Q_UNUSED(id);
        qDebug() <<"在SQL Server中根据id得到User表一条记录";
        return NULL;  //这里为了做例子，所以没有真正返回业务需要的指针，只是返回空指针做示范
    }
};
class AccessUser final: public IUser
{
public:
    void Insert(User user) override
    {
        Q_UNUSED(user);
        qDebug() <<"在Access中给User表增加一条记录";
    }
    User* getUser(int id) override
    {
        Q_UNUSED(id);
        qDebug() <<"在Access中根据id得到User表一条记录";
        return nullptr;   //这里为了做例子，所以没有真正返回业务需要的指针，只是返回空指针做示范
    }
};

class Department
{
private:
    QString _departmentName;
};

class IDepartment
{
public:
    virtual void Insert(Department department) = 0;
};
class SqlServerDepartment:public IDepartment
{
public:
    void Insert(Department department) override
    {
        Q_UNUSED(department);
        qDebug() <<"在SQL Server中给Department表增加一条记录";
    }
};
class AccessDepartment:public IDepartment
{
public:
    void Insert(Department department) override
    {
        Q_UNUSED(department);
        qDebug() <<"在Access中给Department表增加一条记录";
    }
};

class IFactory
{
public:
    virtual IUser* CreateUser() = 0;

    virtual IDepartment *CreateDepartment() = 0;
};
class SqlServerFactory: public IFactory
{
public:
    IUser* CreateUser() override
    {
        return new SqlServerUser();
    }

    IDepartment* CreateDepartment() override
    {
        return new SqlServerDepartment();
    }
};
class AccessFactory: public IFactory
{
public:
    IUser* CreateUser() override
    {
        return new AccessUser();
    }

    IDepartment* CreateDepartment() override
    {
        return new AccessDepartment();
    }
};
#endif // USER

#include "user.h"

#define use_SQLServer
int main(int argc, char *argv[])
{
    Q_UNUSED(argc); Q_UNUSED(argv);
    User *user = new User();
    Department *department = new Department();
#ifdef use_SQLServer
    IFactory *factory = new SqlServerFactory();
#else
    IFactory *factory = new  AccessFactory();
#endif
    IUser *iu = factory->CreateUser();
    IDepartment *id = factory->CreateDepartment();

    id->Insert(*department);
    iu->getUser(1);
    iu->Insert(*user);

    return 0;
}
```

这样的实现就可以满足同样的代码，如果需要通过不同的数据库实现同样的功能的话，只需要在开头的`#define use_SQLServer`注释掉和不注释掉两种方法即可，大大的增加的软件的复用性，减少了相关的修改操作。