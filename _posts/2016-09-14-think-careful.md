---
layout:     post
title:      "解决昨天加班的问题"
subtitle:   "2016/9/14 仔细 思考"
date:       2016-09-14
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160914.jpg?raw=true"
tags:
    - 日记
    - 设计模式
---

### 时间:2016年9月14日 天气:晴:sunny:

-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    说到昨天的加班，是因为编程的时候遇到一个我怎么也想不到的问题，就是我本来运行的程序在window7系统上面没有问题，但是一到window8以及以上的系统就出问题了。
    我仔细分析程序的源代码，但是从逻辑上没有发现问题！这种错误实在是没办法解决！因此昨天晚上加班到10点多，看电脑屏幕看的头晕眼花也没有找到问题的原因！
    今天早晨，仔细研究了一上午，终于发现了问题！哎，还是自己经验不足，学艺不精啊！明天就是中秋节了，好好补习补习程序吧……
</pre>

今天在工作中遇到的问题是很奇怪的，因为程序出现的问题是在window7系统上面没有问题，但是一到window8以及以上的系统就出问题了。我开始以为是系统兼容性的问题，但是一般window系统出问题的概率比较小，而且真的是系统的问题的话我也无法解决。我又仔细研究了我的代码，在window10的系统上面进行调试。代码每次的定位点都不一样！最后，我把一个函数中的delete语句删除后，问题解决。原来问题竟然是内存重复释放这种基本错误！！

### 设计模式(十四)————抽象工厂模式(续)

上次进行抽象工厂模式的使用时，对于使用哪种数据库来说，还是必须在主函数里面通过新建不同的数据库类来实现不同数据库的访问，如果我想要把这个逻辑也在逻辑代码中实现，就可以使用简单工厂模式改进抽象工厂模式，代码如下：

```C++
#ifndef DATAACCESS_H
#define DATAACCESS_H

#include "user.h"
#include <QString>

class DataAccess
{
public:
    static IUser* CreatUser()
    {
        IUser* result = nullptr;
        if(_db == "Sqlserver")
            result = new SqlServerUser();
        else if(_db == "Access")
            result = new AccessUser();
        return result;
    }
    static IDepartment* CreatDepartment()
    {
        IDepartment* result = nullptr;
        if(_db == "Sqlserver")
            result = new SqlServerDepartment();
        else if(_db == "Access")
            result = new AccessDepartment();
        return result;
    }

private:
    static const QString _db;  //注意：静态非整型数据成员，必须定义为const，且在类外初始化
};

//const QString DataAccess::_db = QString::fromUtf8("Sqlserver");
const QString DataAccess::_db = QString::fromUtf8("Access");   //把上面那句话取消注释，本行加上注释，就是使用sql服务的相关操作
```

这样主函数改为：

```C++
#include "user.h"
#include "dataaccess.h"
int main(int argc, char *argv[])
{
    Q_UNUSED(argc); Q_UNUSED(argv);
    User *user = new User();
    Department *department = new Department();


    IUser *iu = DataAccess::CreatUser();
    IDepartment *id = DataAccess::CreatDepartment();

    id->Insert(*department);
    iu->getUser(1);
    iu->Insert(*user);

    return 0;
}
```

这样，在主函数中使用`DataAccess`的简单工厂来创建出对应的类的实例即可。然后如果需要改的数据库的类型，直接在`dataaccess.h`头文件中将静态变量`_db`的内容改动一下即可。