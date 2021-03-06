---
layout:     post
title:      "接到面试通知"
subtitle:   "2016/9/18 紧张"
date:       2016-09-18
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160918.jpg?raw=true"
tags:
    - 日记
    - 设计模式
    - Qt
---

### 时间:2016年9月18日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天晚上，收到了一个面试通知，让我周二去面试，感觉内心比较紧张。哎，这种在一个单位干活，然后去另一个单位面试的情况，
    我从来没有尝试过，但是还是试一试吧，主要是我从来没有面试过，所以一定要涨涨经验啊。所以我就同意了。哎，明天怎么跟老板请假呢？
</pre>

### 设计模式(十四)————抽象工厂模式(再续)

今天继续研究抽象工厂模式，果然这个模式有难度，但是今天我终于给这个模式搞出来了，非常高兴！虽然使用Qt框架取巧了，但是也算是实现了。

今天我实现的内容是抽象工厂模式的第三次境界。前两次分别是：普通的抽象工厂模式的使用、简单工厂模式改进抽象工厂模式的使用。而今天的第三重境界就是：在简单工厂模式中使用反射技术来改进抽象工厂模式。话不多说，放上代码：

```C++
#ifndef DATAACCESS_H
#define DATAACCESS_H

#include "user.h"

Q_DECLARE_METATYPE(SqlServerUser)
Q_DECLARE_METATYPE(AccessUser)
Q_DECLARE_METATYPE(SqlServerDepartment)
Q_DECLARE_METATYPE(AccessDepartment)
void RegisterMetaType()
{
    qRegisterMetaType<SqlServerUser>();
    qRegisterMetaType<AccessUser>();
    qRegisterMetaType<SqlServerDepartment>();
    qRegisterMetaType<AccessDepartment>();
}
class DataAccess
{
public:
    static IUser* CreatUser()
    {
        IUser* result = nullptr;
        QString className = _db + "User";
        int id = QMetaType::type(className.toLatin1());
         if (id != QMetaType::UnknownType)
             result = static_cast<IUser*>(QMetaType::create(id));
         else
             qDebug()<<"没有在Qt中注册成功该类";
        return result;
    }
    static IDepartment* CreatDepartment()
    {
        IDepartment* result = nullptr;
        QString className = _db + "Department";
        int id = QMetaType::type(className.toLatin1());
         if (id != QMetaType::UnknownType)
             result = static_cast<IDepartment*>(QMetaType::create(id));
         else
             qDebug()<<"没有在Qt中注册成功该类";
        return result;
    }
private:
    static const QString _db;  //注意：静态非整型数据成员，必须定义为const，且在类外初始化
};

#ifdef use_SQLServer
const QString DataAccess::_db = QString::fromUtf8("SqlServer");
#else
const QString DataAccess::_db = QString::fromUtf8("Access");   //把上面那句话取消注释，本行加上注释，就是使用sql服务的相关操作
#endif

#endif // DATAACCESS_H
```

由于我使用了Qt的反射技术，这里讲解一下。C++本身不想java或者C#那样可以有库保护反射的方法。因此只能自己实现通过类名动态的创建类。但是Qt框架的元系统可以使这一过程简化，如果想要动态的在运行时根据字符串的名字创建类，只需要完成以下三步即可：

1. 将需要使用的类通过`Q_DECLARE_METATYPE`注册，如：`Q_DECLARE_METATYPE(CExample)`。注意：（1）需要构造的类必须提供公用的构造函数、拷贝构造函数和析构函数，当然如果没有复杂的需要深拷贝的数据，使用编译器默认提供的也行。（2）以下类无需使用该宏即可自动的注册到Qt元系统中：a.继承自QObject的类；b.
QList<T>, QVector<T>, QQueue<T>, QStack<T>, QSet<T> 和 QLinkedList<T> 这些数据结构中T是 被注册过的类型时；c.
QHash<T1, T2>, QMap<T1, T2> 和 QPair<T1, T2> 这些数据结构中T1和T2是 被注册过的类型时；d.
QPointer<T>, QSharedPointer<T>, QWeakPointer<T>, 这些智能指针中T是被注册过的类型时；e.
通过`Q_ENUM` 或者 `Q_FLAG`注册的枚举数据；f.
具有一个`Q_GADGET` 宏的类。
2. 如果需要动态的根据类名创建该类，需要在main函数中通过`qRegisterMetaType`注册，例如：`qRegisterMetaType<CExample>();` 注意：如果仅仅要在`QVariant`中使用该类，就不需要这一步了。
3. 通过`int id = QMetaType::type(该类名字的字符串转换为QByteArray或者直接const char*)`，然后使用`QMetaType::create(id)`返回一个新的该类的指针。例如：

```
QString className = "CExample";
int id = QMetaType::type(className.toLatin1());  //或者int id = QMetaType::type("CExample"); 
CExample* result = static_cast<CExample*>(QMetaType::create(id));
```

通过上面三步，就可以动态的通过类名创建类啦，我上面的`DATAACCESS_H`文件也是这样用的，接下来，在main函数中的代码如下：

```C++
#define use_reflect   //如果不使用反射，注释掉这句话
#define use_SQLServer   //如果使用AccessServer,注释掉这句话

#include "user.h"
#include "dataaccess.h"

int main(int argc, char *argv[])
{
    Q_UNUSED(argc); Q_UNUSED(argv);
#ifdef use_reflect
    RegisterMetaType();
#endif
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

这样就实现了在简单工厂模式中使用反射技术来改进抽象工厂模式。通过这样的改进，我们可以发现当我们需要使用简单工厂构造多个新的类的时候，只需要修改那个需要变化的字符串，然后其他地方都不需要改动，就可以实现对不同数据库的调用，真的是非常方便，非常好用，设计模式实在是太棒了！

最后放上源码地址：
https://github.com/Dongzhixiao/designMode_qt/tree/master/visitDatabase_Abstract_Factory_15