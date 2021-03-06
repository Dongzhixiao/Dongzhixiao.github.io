---
layout:     post
title:      "使用QTcpSocket接收数据"
subtitle:   "2017/04/06 读取图片数据"
date:       2017-04-06
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170406.jpg?raw=true"
tags:
    - Qt
    - QTcpSocket
---

### 时间:2017年4月6日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

最近，接到一个项目，是连接TCP后读取一个图片的数据，对方为了简化，直接发送数据的二进制格式，因此我必须连接后直接读取，然后在读取完成后进行保存。因为数据没有在开头加入大小信息，而TCP传输的时候并没有一次全部传输完成，只是默认传输一定量的数据，因此我必须时刻检验数据是否全部读取完毕，然后显示图片。

最后，我没有想明白如何控制读取内容一次全部读取完成，只能使用笨办法，每次读取一段数据就将数据加入所有数据中，然后实时显示，发现显示的效果是一张图片缓缓地出现，虽然慢了点，但是实现功能了，具体的改进等我想好好的方法再加上。
复制显示的头文件代码如下：
```C++
#ifndef DIALOG_TEST_PIC_H
#define DIALOG_TEST_PIC_H

#include <QDialog>

namespace Ui {
class Dialog_test_pic;
}

class Dialog_test_pic : public QDialog
{
    Q_OBJECT

public:
    explicit Dialog_test_pic(QWidget *parent = 0);
    ~Dialog_test_pic();

private slots:
    void on_pushButton_connect_clicked();  //!< @brief 单击连接按钮进入的槽函数
    //TCP图像设备数据处理相关槽函数
    void visualTcp_readClient(void);  //!< @brief 读取图像设备TCP反馈数据的槽函数 @details 在图像设备TCP建立连接后其@a readyRead() 信号触发时进入的槽函数
    void visualTcp_OnConnected(void);  //!< @brief 读取图像设备TCP反馈数据的槽函数 @details 在图像设备TCP建立连接后其@a connected() 信号触发时进入的槽函数

    void on_pushButton_save_clicked();  //!< @brief 单击保存按钮进入的槽函数

    void test();  //!< @brief 跟新图片并显示的槽函数 @details

private:
    Ui::Dialog_test_pic *ui;
//    QSharedPointer<QImage> pic;   //!< 保存图像的指针
    QByteArray picArray;   //!< @brief 保存图片的字节数组
    QImage pic;            //!< @brief 保存图片并负责保存的图片数据
};

#endif // DIALOG_TEST_PIC_H
```
源文件代码如下：
```C++
#include "dialog_test_pic.h"
#include "ui_dialog_test_pic.h"
#include <QTcpSocket>
#include <QHostAddress>
#include <QDateTime>
#include <QFileDialog>

#include <QDebug>

Dialog_test_pic::Dialog_test_pic(QWidget *parent) :
    QDialog(parent),
    ui(new Ui::Dialog_test_pic)
{
    ui->setupUi(this);
}

Dialog_test_pic::~Dialog_test_pic()
{
    delete ui;
}

void Dialog_test_pic::on_pushButton_connect_clicked()
{
    picArray.clear();  //点击按钮之后重新连接，清空原来读取的数据
    //新建tcp接口
    QTcpSocket *visualTcpSocket = new QTcpSocket(this);
    //加入信号和槽
    connect(visualTcpSocket, SIGNAL(readyRead()), this, SLOT(visualTcp_readClient()));
    connect(visualTcpSocket, SIGNAL(connected()), this, SLOT(visualTcp_OnConnected()));
    connect(visualTcpSocket, SIGNAL(disconnected()), visualTcpSocket, SLOT(deleteLater()));
//    connect(visualTcpSocket,SIGNAL(readChannelFinished()),this,SLOT(test()));
    //TCP连接(直接加上地址和端口)
    visualTcpSocket->connectToHost(ui->lineEdit_IP->text(), ui->lineEdit_port->text().toInt());  //连接摄像头的socket
    ui->label_image_show->setText("正在尝试连接显示设备，请稍等...");
}

void Dialog_test_pic::test()
{
    qDebug()<<"读取完成";
    qDebug()<<"全部数据是："<<picArray.size();
    pic = QImage::fromData(picArray);
    QImage resultImg = pic.scaled(ui->label_image_show->size(),Qt::KeepAspectRatio,Qt::SmoothTransformation);
    ui->label_image_show->setPixmap(QPixmap::fromImage(resultImg));
//    qDebug()<<"全部数据是："<<picArray.data();
}

void Dialog_test_pic::visualTcp_OnConnected()
{
    ui->label_image_show->setText("连接成功，正在等待图片数据");
}

void Dialog_test_pic::visualTcp_readClient()
{
    QTcpSocket *socket = qobject_cast<QTcpSocket*>(sender());
//    qDebug()<<"当前读取前可以读取数据的字节是："<<socket->bytesAvailable();
    QByteArray datagram = socket->readAll();
//    qDebug()<<datagram.data();
//    pic = QSharedPointer<QImage>(new QImage(datagram.data()));
    picArray.append(datagram);
    test();
}

void Dialog_test_pic::on_pushButton_save_clicked()
{
    QDateTime time = QDateTime::currentDateTime();//获取系统现在的时间
    QString str = time.toString(tr("hh时mm分")); //设置显示格式
    QString fileName = QFileDialog::getSaveFileName(this,tr("保存文件"),str,tr("bmp文件(*.bmp)"));
    if(fileName.length()==0)
        return;
    QFile imageFile(fileName);
    if (!imageFile.open(QIODevice::WriteOnly))
        return;
//    QDataStream dataStream(&imageFile);
//    dataStream<<pic;
    pic.save(&imageFile);
    qDebug()<<fileName;

}
```

在保存的时候直接使用QImage类自带的save()方法，不要使用QDataStream，因为Qt的数据流会在开头加入数据大小信息，保存后就无法打开标准图片数据了。

完整代码在：
[https://github.com/Dongzhixiao/C-_study_onQT/tree/master/test_pic](https://github.com/Dongzhixiao/C-_study_onQT/tree/master/test_pic)