---
layout:     post
title:      "和姐姐通话"
subtitle:   "2016/9/6 十一 回家"
date:       2016-09-06
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160906.jpg?raw=true"
tags:
    - 日记
    - Qt
    - Effect C++
    - 设计模式
---

### 时间:2016年9月6日 天气:小雨:umbrella:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天是周二，田田姐姐又给我打电话问我情况啦，听说她要十一回家，一想到到时
候能见面，真的是很高兴！期待ing
</pre>

今天在工作中遇到网络连接的问题，普通的客户端网络连接用Qt实现很简单，就是新建一个`QTcoSocket/QUdpSocket`，
然后输入端口号和ip地址，建立三个信号和槽(连接、断开、读取)实现对应的槽函数即可。

但是今天遇到的问题要求是在同一个网络里面有好的服务器，而且每一个服务器发送的UDP的IP地址和端口号相同，
唯一识别它们不同的地方在于连接上UDP后发送的数据里面有TCP的地址，每一个服务器发送的tcp地址的不同是区别它们的唯一方式。
我需要做的就是在这个网段上搜索到这些服务器发送的不同tcp地址，然后根据地址列出表格，然后让用户自己选择连接的对应控制器。

我经过仔细思考，感觉只能通过不断发送和断开UDP连接，然后将所有搜到的信息列出来，最后让客户选择想要连接的那个服务器。
最后我写了一个测试代码，效果不错。关键代码如下：

```C++
#ifndef GUI_SEARCHING_H
#define GUI_SEARCHING_H

#include <QDialog>
#include "robot_info.h"

class QTcpSocket;
class QUdpSocket;

namespace Ui {
class gui_searching;
}

class gui_searching : public QDialog
{
    Q_OBJECT

public:
    explicit gui_searching(QWidget *parent = 0);
    ~gui_searching();

private slots:
    void net_deal_init_search();
    void processPendingDatagrams_search();
    void udp_robot_state_get(QJsonDocument *doc_p, ROBOT_CONTROL_INFO *info_p);

    void on_pushButton_searching_clicked();
    void updateListWidget();

    void on_listWidget_result_doubleClicked(const QModelIndex &index);

private:
    Ui::gui_searching *ui;

    //! 定义网络链接
    QTcpSocket *tcpSocket = NULL;
    QUdpSocket *udpSocket = NULL;

    //! 机器人控制器信息
    QList <ROBOT_CONTROL_INFO> list_robot_control_info;

    bool testIsContainIP(const QString &);
};

#endif // GUI_SEARCHING_H




#include <QTcpSocket>
#include <QUdpSocket>
#include <QMessageBox>

#include <QDebug>

gui_searching::gui_searching(QWidget *parent) :
    QDialog(parent),
    ui(new Ui::gui_searching)
{
    ui->setupUi(this);

}

gui_searching::~gui_searching()
{
    delete ui;
}

void gui_searching::net_deal_init_search()
{
    if(udpSocket && udpSocket->isOpen())
    {
        qDebug()<<"断开了UDP连接";
        udpSocket->leaveMulticastGroup(QHostAddress(UDP_IP));
        udpSocket->close();
        udpSocket->deleteLater();
        udpSocket = NULL;
    }
    udpSocket = new QUdpSocket(this);
    QHostAddress mcast_addr(UDP_IP);
    udpSocket->bind(QHostAddress::AnyIPv4, UDP_PORT, QUdpSocket::ShareAddress | QUdpSocket::ReuseAddressHint);
   //加入组播地址
    udpSocket->joinMulticastGroup(mcast_addr);
    connect(udpSocket, SIGNAL(readyRead()), this, SLOT(processPendingDatagrams_search()));
//    udp_frame_tmp.resize(0);

}

//读取UDP反馈数据
void gui_searching::processPendingDatagrams_search()
{

    int robot_control_num = -1;  //已连接上位机的控制器编号
    if (udpSocket->hasPendingDatagrams())
    {  //至少一条数据已经准备被读取
        QByteArray datagram;   //存储收到的数据
        datagram.resize(udpSocket->pendingDatagramSize());
        QHostAddress sender;   //存储发送者的UDP的IP地址
        quint16 senderPort;    //存储发送者的端口号
        udpSocket->readDatagram(datagram.data(), datagram.size(), &sender, &senderPort);

        QJsonParseError *error = new QJsonParseError;
        QJsonDocument doc = QJsonDocument::fromJson(datagram, error);
        if (error->error == QJsonParseError::NoError)
        {
            //解析UDP信息
            ROBOT_CONTROL_INFO info;
            info.socket = tcpSocket;
            info.ip = sender.toString();
            info.link = RETURN_UNKNOWN;
            udp_robot_state_get(&doc, &info);
            if(!testIsContainIP(info.udp_state.ip))
            {
                list_robot_control_info.append(info);
                qDebug()<<"id:" << info.udp_state.id  << "ip : " << info.udp_state.ip;
                updateListWidget();
            }
//            udpSocket->leaveMulticastGroup(QHostAddress(UDP_IP));
//            udpSocket->close();
//            udpSocket->deleteLater();
//            udpSocket = NULL;
//            net_deal_init_search();
        }
        else
            qDebug() << "<" + sender.toString() << tr("> UDP数据解析错误: ") << error->errorString();
        delete error;
    }
}

//获取UDP上报信息
void gui_searching::udp_robot_state_get(QJsonDocument *doc_p, ROBOT_CONTROL_INFO *info_p)
{
    QJsonObject obj = doc_p->object();
    info_p->udp_state.id = obj["id"].toString();
    info_p->udp_state.ip = obj["IP"].toString();
    info_p->udp_state.robot_state = obj["robot_state"].toInt();
    info_p->udp_state.op_state = obj["op_state"].toInt();
    info_p->udp_state.en_state = obj["en_state"].toInt();
    info_p->udp_state.servo_state = obj["servo_state"].toInt();
    info_p->udp_state.motion_state = obj["motion_state"].toInt();
    info_p->udp_state.comm_state = obj["comm_state"].toInt();
    QJsonArray list = obj["position"].toArray();
    for (int i = 0; i < list.count(); i++)
        info_p->udp_state.list_position.append(list.at(i).toInt());
    list = obj["axisPosition"].toArray();
    for (int i = 0; i < list.count(); i++)
        info_p->udp_state.list_axisPosition.append(list.at(i).toInt());
    info_p->udp_state.speed = obj["speed"].toDouble();
    QJsonObject obj_io = obj["IO"].toObject();
    list = obj_io["DI"].toArray();
    for (int i = 0; i < list.count(); i++)
        info_p->udp_state.list_io_di.append(list.at(i).toInt());
    list = obj_io["DO"].toArray();
    for (int i = 0; i < list.count(); i++)
        info_p->udp_state.list_io_do.append(list.at(i).toInt());
    QJsonObject obj_stack = obj["stack"].toObject();
    info_p->udp_state.stack_project = obj_stack["Project"].toString();
    info_p->udp_state.stack_file = obj_stack["File"].toString();
    info_p->udp_state.stack_cn = obj_stack["CN"].toString();
    info_p->udp_state.stack_func_start_line = obj_stack["FuncStartLine"].toInt();
    info_p->udp_state.stack_pc = obj_stack["PC"].toInt();
}

void gui_searching::on_pushButton_searching_clicked()
{
    //检查rivo-stdio是否和控制器有链接
    for (int i = 0; i < list_robot_control_info.count(); i++)
    {
        if (list_robot_control_info.at(i).socket &&
            list_robot_control_info.at(i).socket->isOpen())
        { //如果当前连接有控制器，提醒客户是否断开连接，因为搜索时必须先断开连接！
            QString IP = list_robot_control_info.at(i).ip;
            if(QMessageBox::warning(this,tr("警告"),tr("当前正连接着ip是：1%的控制器，搜索前必须断开连接，"
                                                     "请是否确定断开连接？").arg(IP),QMessageBox::Yes | QMessageBox::No)
               == QMessageBox::Yes)
            {
                if(udpSocket && udpSocket->isOpen())
                {
                    qDebug()<<"断开了UDP连接";
                    udpSocket->leaveMulticastGroup(QHostAddress(UDP_IP));
                    udpSocket->close();
                    udpSocket->deleteLater();
                    udpSocket = NULL;
                }
                list_robot_control_info.at(i).socket->close();
                list_robot_control_info.at(i).socket->deleteLater();
                list_robot_control_info[i].socket = nullptr;
                list_robot_control_info[i].link = RETURN_UNKNOWN;
                break;
            }
            else
                return;
        }
    }
    ui->listWidget_result->clear();
    list_robot_control_info.clear();
    net_deal_init_search();
}

void gui_searching::updateListWidget()
{
    int count = ui->listWidget_result->count();
    qDebug()<<"当前listWidget里面的数目："<<count;
    if(count+1 == list_robot_control_info.count())
        ui->listWidget_result->addItem(list_robot_control_info.at(count).udp_state.ip);
}

bool gui_searching::testIsContainIP(const QString & s)
{
    for(int i = 0 ; i != list_robot_control_info.count() ; ++i)
        if(list_robot_control_info.at(i).udp_state.ip == s)
            return true;
    return false;
}

void gui_searching::on_listWidget_result_doubleClicked(const QModelIndex &index)
{
    qDebug()<<"进入了双击的槽函数，现在选择的ip是："
           <<index.data().toString();

    //TCP初始化
    tcpSocket = new QTcpSocket(this);
    connect(tcpSocket, SIGNAL(readyRead()), this, SLOT(tcp_readClient()));
    connect(tcpSocket, SIGNAL(connected()), this, SLOT(tcp_OnConnected()));
    connect(tcpSocket, SIGNAL(disconnected()), this, SLOT(tcp_OnDisconnected()));
    //将socket加入到list里面
    for(int i = 0 ; i != list_robot_control_info.count() ; ++i)
        if(list_robot_control_info.at(i).udp_state.ip == index.data().toString())
        {
            list_robot_control_info[i].socket = tcpSocket;
            break;
        }
    tcpSocket->connectToHost(QHostAddress(index.data().toString()), TCP_PORT);
    this->accept();
}
```

里面通过一个全局的链表存储每次读取数据的中UDP传输过来的TCPIP并存储，然后当里面没有相同的TCPIP地址时加入进去，然后断开UDP，然后再次连接，这样一直重复，理论上能被所有服务器的ip都提取出来。

#### 条款23 :宁以non-member、non-friend替换member函数 

本条款还是讨论封装的问题，举书上的例子：

```C++
class WebBrower
{
public:
    void ClearCach();
    void ClearHistory();
    void RemoveCookies();
};
```

定义了一个WebBrower的类，里面执行对浏览器的清理工作，包括清空缓存，清除历史记录和清除Cookies，现在需要将这三个函数打包成一个函数，这个函数执行所有的清理工作，那是将这个清理函数放在类内呢，还是把他放在类外呢？

如果放在类内，那就像这样：

```C++
class WebBrower
{
    …
    void ClearEverything()
    {
        ClearCach();
        ClearHistory();
        RemoveCookies();
    }
    …
};
```

如果放在类外，那就像这样：

```C++
void ClearWebBrowser(WebBrower& w)
{
    w.ClearCach();
    w.ClearHistory();
    w.RemoveCookies();
}
```

根据面向对象守则的要求，数据以及操作数据的函数应该捆绑在一起，都放在类中，这意味着把它放在类内会比较好。但从封装性的角度而言，它却放在类外好，为什么？

为了区分开，我们把在类内的总清除函数称之为ClearEverything，而把类外的总清除函数称之为ClearWebBrower。ClearEverything对封装性的冲击更大，因为它位于类内，这意味着它除了访问这三个公有函数外，还可以访问到类内的私有成员，是的，你也许会说现在这个函数里面只有三句话，但随着功能的扩充，随着程序员的更替，这个函数的内容很可能会逐渐“丰富”起来。而越“丰富”说明封装性就会越差，一旦功能发生变更，改动的地方就会很大。

再回过头来看看类外的实现，在ClearWebBrowser()里面，是通过传入WebBrower的形参对象来实现对类内公有函数的访问的，在这个函数里面是绝对不会访问到私有成员变量（编译器会为你严格把关）。因此，ClearWebBrowser的封装性优于类内的ClearEverything。

但这里有个地方需要注意，ClearWebBrower要是类的非友元函数，上面的叙述才有意义，因为类的友元函数与类内成员函数对封装性的冲击程度是相当的。

看到这里，你也许会争辩，把这个总清除的功能函数放在类外，就会割离与类内的关联，逻辑上看，这个函数就是类内函数的组合，放在类外会降低类的内聚性。

为此，书上提供了命名空间的解决方案，事实上，这个方案也是C++标准程序库的组织方式，好好学习这种用法很有必要！像这样：

```C++
namespace WebBrowserStuff
{
    …
    class WebBrowser();
    void ClearWebBrowser(WebBrowser& w);
    …
}
```

namespace与class不同，前者可以跨越多个源码文件，而后者不能。通过命名空间的捆绑，是在封装和内聚之间非常好的平衡。

更有意思的是，与WebBrower相关的其他功能，比如书签、cookie管理等等，他们与WebBrower紧密相关，但放在单看书签和cookie，两者又是逻辑不同的，喜欢书签管理的用户不一定喜欢cookie管理，这时如果把他们统统放在类内，会显得有些不妥。书上提供了很好的命名空间组织方式，像这样：

```C++
// 头文件webbrowser.h
namespace WebBrowserStuff
{
    class WebBrowser(); // 核心功能
    void ClearWebBrowser(WebBrowser& w); // non-member non-friend函数
}

// 头文件webbrowserbookmarks.h
namespace WebBrowserStufff
{
    …// 与书签相关的函数
}

// 头文件 webbrowsercookies.h
namespace WebBrowserStuff
{
    …// 与cookie管理相关的函数
}
```

将他们放在不同的头文件中，但位于同一个namespace中，是内聚与封装平衡的极佳处理方式。

C\+\+标准库中，容器是std命名空间的重要组成部分，但容器也有好多种，比如vector，set和map等等，如果把它们写在一个头文件中，会很臃肿。C\+\+把它们放在不同的头文件中，但又位于同一个命名空间里，方便用户使用，比如用户只想用vector，那么只要包含#include <vector>就行了，而不必#include <set>，并且一句using namespace std即可不加前缀地使用其内定义的各种名称。

最后总结一下，本条款强调封装性优先于类的内聚逻辑，这是因为“愈多东西被封装，愈少人可以看到它，而愈少人看到它，我们就有愈大的弹性去改变它，因为我们的改变仅仅影响看到改变的那些人或事物”。采用namespace可以对内聚性进行良好的折中。

请记住： 

- [x] ***尽量用non-member non-friend函数替换member函数。可以增加封装性、包裹弹性（packaging flexibility）、和机能扩充性。***


#### 设计模式(八)————原型模式

用原型实例指定创建对象的种类，并且通过拷贝这些原型创建新的对象!
原型模式其实就是从一个对象再创建另外一个可定制的对象，而且不需要知道任何创建的细节！！
一下实现一个简历的例子：

```C++
#ifndef RESUME
#define RESUME
#include <QString>
#include <QtDebug>

class Resume
{
public:
    explicit Resume(QString name):_name(name){}
//    Resume(const Resume & r) //也可以自己实现拷贝构造函数
//    {_name = r._name;_sex=r._sex;_age=r._age;_timeArea=r._timeArea;_company=r._company;}
    void setPersonalInf(QString sex, QString age)
    {
        _sex = sex;
        _age = age;
    }
    void setPersonalExperience(QString timeArea, QString company)
    {
        _timeArea = timeArea;
        _company = company;
    }
    void display()
    {
        qDebug() << _name<<","<<_sex<<","<<_age;
        qDebug() <<"工作经历："<<_timeArea <<_company;
    }
    Resume* clone() {return new Resume(*this);}  //这里返回使用默认拷贝构造函数的结果，因此每个值都拷贝了，没有指针，因此浅拷贝就行！
private:
    QString _name;
    QString _sex;
    QString _age;
    QString _timeArea;
    QString _company;
};

#endif // RESUME

#ifndef DEEPRESUME
#define DEEPRESUME
#include <QString>
#include <QtDebug>

class WorkExperience
{
public:
    explicit WorkExperience(QString ta,QString c):_timeArea(ta),_company(c){}
    QString getTimeArea(){return _timeArea;}
    QString getCompney() {return _company;}
private:
    QString _timeArea;
    QString _company;
};

class DeepResume
{
public:
    explicit DeepResume(QString name):_name(name){}
    DeepResume(const DeepResume & r) //自己实现拷贝构造函数,注意第二行实现的深拷贝！！
    //注意：拷贝构造函数可以调用引用参数对象的私有成员！！
    //因为拷贝构造函数是放在本身这个类里的，而类中的函数可以访问这个类的对象的所有成员，当然包括私有成员了。
    {   _name = r._name;_sex=r._sex;_age=r._age;
        _we = new WorkExperience(*r._we);
    }
    ~DeepResume()
    {
        if(_we != nullptr)
        {
            delete _we;
            _we = nullptr;
        }
    }
    void setPersonalInf(QString sex, QString age)
    {
        _sex = sex;
        _age = age;
    }
    void setWorkExperience(WorkExperience *we)
    {_we = we;}
    void display()
    {
        qDebug() << _name<<","<<_sex<<","<<_age;
        qDebug() <<"工作经历："<<_we->getTimeArea()<<_we->getCompney();
    }
    DeepResume* clone() {return new DeepResume(*this);}  //这里返回使用默认拷贝构造函数的结果，因此每个值都拷贝了，有指针，因此深拷贝才行！
private:
    QString _name;
    QString _sex;
    QString _age;
    WorkExperience * _we = nullptr;
};

#endif // DEEPRESUME

#include "resume.h"
#include "deepresume.h"
#include <QtDebug>
#include <iostream>
using namespace std;

int main(int argc, char *argv[])
{
    //浅拷贝的原型模式
    Resume *xd=new Resume("小东");
    xd->setPersonalExperience("2015-2016","恒通");
    xd->setPersonalInf("male","27");
    Resume xdd(*xd);
    xdd.display();
    xd->display();
    //深拷贝的原型模式
    DeepResume *xc = new DeepResume("小崔");
    xc->setPersonalInf("male","28");
    xc->setWorkExperience(new WorkExperience("2000-2001","电力"));
    DeepResume xcc(*xc);
    delete xc;   //因为使用了深拷贝，因此这里就算删除了xc，下面的函数依然可以正常运行，调用里面的WorkExperience类
    xcc.display();
    //xc->display();
    return 0;
}
```

使用原型模式的核心是实现clone方法，我使用了浅拷贝和深拷贝两种方式来实现，分别写出了对应的clone方法的实现。
