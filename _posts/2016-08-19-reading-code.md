---
layout:     post
title:      "Qt界面问题"
subtitle:   "2016/08/19 阅读别人代码学习"
date:       2016-08-19
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160819.jpg?raw=true"
tags:
    - 日记
    - Qt
---

### 时间:2016年8月19日 天气:晴:sunny:
-----
#####   Author:冬之晓:dizzy_face:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天，我在进行Qt编程的时候，关于界面的编制遇到了一些问题，如果一个ComboBox控件的选项直接有关联的话，
	如何在已经选择的选项前面加上提示，如何动态的改变ComboBox，如何使用右键菜单。在读别人的代码的过程中发现了这些，
	特此记录一下，以防忘记。
</pre>

为了将所有例子都在一个程序中表示出来，我设计一个简单的对话框，其特点是可以使用右键菜单进行增加条目，如图：

![增加条目](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/dialogAdd.png "增加条目")

也可以在对应的条目上右键进行删除，如图：

![删除条目](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/dialogDelete.png "删除条目")

还可以修改每个条目的名字，同时增加几个条目右边的comboBox会随时变化变为对应的数目，而且选择的时候可以在已经选择过的条目出现叉号提示，如图：

![提示](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/dialogHint.png "提示")

为了实现上述功能，我按照以下的几个步骤进行：

#### 程序ui界面的添加

Qt之所以非常适合编写界面程序，是因为它可以方便的进行拖拽控件进行界面逻辑的制定。为了可以使用多个条目，我在程序整体框架上拖拽一个`QTreeWidget`控件，这个控件允许生成很多子条目，每个子条目上面可以根据不同的列添加对应的控件。然后再添加`QAction`，我觉得action是一个程序的核心，特别是界面编程，只要把每一个动作的业务逻辑对应好，在主界面的菜单栏和工具栏就可以直接拖动。同时在主界面的中央控件上的右键菜单内部也可以添加动作。本例比较简单，只加上“增加”和“删除”两个动作就好。

#### 自定义comboBox

为了实现comboBox控件单击时显示被选中的条目有提示，同时comboBox自己没有按下时响应的信号，因此我只能自己重新实现一个按下时触发的信号，实现时通过实现其父类QWidget的虚函数mousePressEvent(QMouseEvent *event)来响应按压事件，并发送对应信号，的具体实现代码如下：

```C++
class ComboBox:public QComboBox
{
    Q_OBJECT
public:
    explicit ComboBox(QWidget *parent = Q_NULLPTR):QComboBox(parent)
    {}
protected:
    void mousePressEvent(QMouseEvent *e) override
    {
        emit mousePressed();
        QComboBox::mousePressEvent(e);
    }
signals:
    void mousePressed();
};
```

#### 主界面

在实现主界面时，需要实现的槽函数和变量如下：

```C++
class Dialog : public QDialog
{
    Q_OBJECT

public:
    explicit Dialog(QWidget *parent = 0);
    ~Dialog();

private slots:
    void on_treeWidget_customContextMenuRequested(const QPoint &pos);

    void on_action_add_triggered();

    void on_action_delete_triggered();

    void on_treeWidget_itemDoubleClicked(QTreeWidgetItem *item, int column);

    void on_treeWidget_itemClicked(QTreeWidgetItem *item, int column);

    void on_comboBox_changed();
private:
    Ui::Dialog *ui;
    QTreeWidgetItem * _lastItem = nullptr;
    int _lastColumn = 0;
    QList<ComboBox *> allComboBox;
};
```

源文件中为了使用右键菜单，需要响应customContextMenuRequested槽，同时还要设置好treeWidget的内容菜单策略为Qt::CustomContextMenu，其实现代码如下：

```C++
Dialog::Dialog(QWidget *parent) :
    QDialog(parent),
    ui(new Ui::Dialog)
{
    ui->setupUi(this);
    ui->treeWidget->setHeaderLabels(QStringList()<<tr("名称")<<tr("选项"));   //增加条目标题
    ui->treeWidget->setContextMenuPolicy(Qt::CustomContextMenu);   //使得该控件可以支持右键菜单
}

void Dialog::on_treeWidget_customContextMenuRequested(const QPoint &pos)
{
    QMenu *popMenu = new QMenu(this);//定义一个右键弹出菜单
    QTreeWidgetItem *curItem=ui->treeWidget->itemAt(pos);
    if (curItem == NULL) //空白处点击
    {
        popMenu->addAction(ui->action_add);
        ui->treeWidget->closePersistentEditor(_lastItem,_lastColumn);
        _lastItem = nullptr;
        _lastColumn = 0;
    }
    else
        popMenu->addAction(ui->action_delete);

    popMenu->exec(QCursor::pos());//弹出右键菜单，菜单位置为光标位置
}
```

接下来需要实现增加和删除动作。代码如下：

```C++
void Dialog::on_action_add_triggered()
{
    QTreeWidgetItem *child = new QTreeWidgetItem(ui->treeWidget);
    child->setText(0,tr("请修改"));
    ComboBox *comb = new ComboBox(this);
    connect(comb,SIGNAL(mousePressed()),this,SLOT(on_comboBox_changed()));
    //comb->addItems(QStringList()<<"条目1"<<"条目2");
    ui->treeWidget->setItemWidget(child,1,comb);
    allComboBox.append(comb);    //增加对应的ComboBox
    //修改所有comboBox
    int ComNum = allComboBox.count();
    for(int i=0 ; i != ComNum; ++i)
    {
        ComboBox *tem = allComboBox.at(i);
        int temNum = tem->currentIndex(); //保存当前选择的条目数
        tem->clear();
        for(int j = 0 ;j != ComNum ; ++j)
            tem->addItem(QString("条目%1").arg(j+1));
        tem->setCurrentIndex(temNum);
    }
}

void Dialog::on_action_delete_triggered()
{
    QTreeWidgetItem *curItem = ui->treeWidget->currentItem();
    ComboBox *curCom = (ComboBox *)ui->treeWidget->itemWidget(curItem,1);
    for(int i = 0 ; i != allComboBox.count() ;  ++i)
        if(allComboBox.at(i) == curCom)
        {
            allComboBox.removeAt(i);     //删除对应的ComboBox
            break;   //注意，这个不要忘记加！！！！否则会越界！
        }
    delete curItem;
    //修改所有comboBox
    int ComNum = allComboBox.count();
    for(int i=0 ; i != ComNum; ++i)
    {
        ComboBox *tem = allComboBox.at(i);
        int temNum = tem->currentIndex(); //保存当前选择的条目数
        tem->clear();
        for(int j = 0 ;j != ComNum ; ++j)
            tem->addItem(QString("条目%1").arg(j+1));
        tem->setCurrentIndex(temNum);   //设置超过条目数目也没事！！Qt果然厉害
    }
}
```

之后可以出来双击修改条目名字的代码，其中`_lastColumn`和`_lastItem`记录上一次鼠标点击的item和对应的列，这样才能保证在点击合适的位置取消编辑条目名字的时机，代码如下：

```C++
void Dialog::on_treeWidget_itemDoubleClicked(QTreeWidgetItem *item, int column)
{
    if(column == 0)
        ui->treeWidget->openPersistentEditor(item,column);
    _lastColumn = column;
    _lastItem = item;
}

void Dialog::on_treeWidget_itemClicked(QTreeWidgetItem *item, int column)
{
    if(_lastColumn != column || _lastItem != item)
        ui->treeWidget->closePersistentEditor(_lastItem,_lastColumn);
    _lastColumn = column;
    _lastItem = item;
}
```

最后就要实现点击时已经被选中的项目有叉号提示，实际就是有一个全局链表存着所有当前comboBox，这样点击时把所有已经选过的条目记录下来，在当前的comboBox上面对应的条目前面加上一个提示图标就行，代码如下：

```C++
void Dialog::on_comboBox_changed()
{
    //Q_UNUSED(curNum);
    ComboBox * temCom = (ComboBox *)qobject_cast<ComboBox *>(sender());
    QIcon empty;  //定义空图标
    QList<int> tem;
    for(int i = 0 ; i != allComboBox.count() ;  ++i)
        if(allComboBox.at(i) != temCom)
            tem.append(allComboBox.at(i)->currentIndex());
    //qDebug()<<tem.count();
    for(int i = 0 ; i != allComboBox.count() ;  ++i)
        if(tem.contains(i))
            temCom->setItemIcon(i,QIcon(":/Pic/delete_item.png"));
        else
            temCom->setItemIcon(i,empty);
}
```

这样，一个完整的小例子就完成了。总结一下，这个例子学到了

- [x] ***自定义comboBox处理事件***  
- [x] ***QTreeWidget的使用***
- [x] ***右键菜单的使用与动作的添加***
- [x] ***comboBox的图标和条目动态变化***
- [x] ***处理QTreeWidget子条目QTreeWidgetItem的对应列的文字编辑功能***