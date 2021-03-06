---
layout:     post
title:      "去公司加班"
subtitle:   "2016/9/24 周六加班"
date:       2016-09-24
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160924.jpg?raw=true"
tags:
    - 日记
    - Qt
    - qsciscintilla总结
---

### 时间:2016年9月24日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

<pre>
    今天本来以为没事干，结果被要求要加班，因此就来到公司，一直加班到晚上很晚。明明是周六，比平时下班更晚，
    本来今天打算整理一下这一周的进度，但是现在累的不想动了！
</pre>

今天就整理一下Qt下使用qsciscintilla的心得吧。

#### 什么是qsciscintilla

Scintilla是一个免费、跨平台、支持语法高亮的编辑控件。它完整支持源代码的编辑和调试，包括语法高亮、错误指示、代码完成（code completion）和调用提示(call tips)。能包含标记（marker）的页边（margin）可用于标记断点、折叠和高亮当前行。

而QScintilla是Scintilla在QT上的移植。如果想在Qt上面使用强大的Scintilla控件，就安装QScintilla吧！

#### qsciscintilla的下载和安装

首先，你可以到[这个网址](https://riverbankcomputing.com/software/qscintilla/download "qsciscintilla的下载地址")进行下载，然后解压后，可以看到里面有很多文件，其中安装的目录和源码在`Qt4Qt5`文件夹下，在这个文件夹里面使用qmake运行里面的.pro文件，就可以生成makefile文件，然后使用make或者nmake工具运行makefile文件即可。就拿我的Qt举例子，我的Qt安装的是`Qt5.7 MinGW`版本，因此首先在系统环境变量PATH添加你的Qt的qmake路径和mingw32-make路径，比如的的电脑上添加的这两个路径分别是`G:\Qt\Qt5.7.0\5.7\mingw53_32\bin`和`G:\Qt\Qt5.7.0\Tools\mingw530_32\bin`，然后在刚才的`Qt4Qt5`文件夹下面，打开cmd到这个文件夹，分别执行如下语句：

```bat
qmake qscintilla.pro     #使用qmake工具生成makefile文件
mingw32-make             #使用Qt5.7 MinGW版本自带的make工具按照makefile里面的定义的方式编译qscintilla的源文件
mingw32-make install     #将编译生成的库整合到自己的Qt源文件中
```

这样就完成了安装。

#### 简单的例子程序和讲解

这时，你可以打开和`Qt4Qt5`在同一个目录下的`example-Qt4Qt5`文件夹下面有一个例子，运行成功就说明安装成功啦。这个例子运行后，可能会让你大失所望，因为这个例子仅仅实现了打开文件，复制、粘贴、撤销、全选等基本功能。使用Qt自带的`QTextEdit`控件的一推槽函数完全能够实现，甚至能实现更多功能，何必非要安装这个第三方库来做这么麻烦的事情呢？这样想你就错啦，这个例子仅仅展示了qsciscintilla一小部分功能！让我再次基础上稍微加点东西，你就可以看出来这个控件的强大之处啦。

##### 1. 增加关键字定制颜色

如果想要实现关键字定制颜色，想用`QTextEdit`可要花费一番功夫，但是使用`qsciscintilla`就非常简单啦，只要按照以下步骤即可：

a 定义一个所要使用的语言的Lexer(词法分析器)；
b 调用Lexer对应的setColor方法；
c 将Lexer加载到Qscilexer中；

上面的Lexer和Qscilexer都是调用对应语言的相关类，本身类名并不是这样的，就拿C++关键字定制颜色举个例子：

```C++
QsciScintilla *textEdit = new QsciScintilla;
QscilexerCpp *textLexer = new QscilexerCpp;
textLexer->setColor(QColor(Qt:: yellow),QsciLexerCPP::CommentLine);    //设置自带的注释行为绿色
textEdit->setLexer(textLexer);
```

这样就可以将`textEdit`控件中的C++方式的注释行标记为黄色。

当然，除了注释行，`QscilexerCpp`将其中的所有C++相关的关键字都定义到`QsciLexerCPP`类中的一个枚举中。只要在setColor中的第一个参数指明需要的颜色，第二个参数传入对应需要变成这种颜色的枚举中定义的名字即可。

当然，你可能并不满足仅仅改变其内部定义好的关键字的颜色，如果你想让你自定义的字符串改变颜色，这要如何做呢？在`QscilexerCpp`的文档中你可以找到答案。下面简单说一下步骤：

a 继承对于的`lexer`类，实现里面的`keywords(int set)`函数。
b 使用你继承的`lexer`代替前面用的`lexer`类。
c 调用对应的setColor方法；

这里一定要注意`keywords(int set)`函数的实现，一定要是说明文档中枚举类的`KeywordSet`说名对于set的数字实现。就拿`QscilexerCpp`当例子，你查看说文档中对应类枚举，找到其中的`KeywordSet2 `上面的解释是：A keyword defined in keyword set number 2. The class must be sub-classed and re-implement keywords() to make use of this style.(定义在关键字函数集合数字2里面的关键字。如果想使用这种风格， 你的类必须作为该类的子类，并且实现`keywords()`函数)。

安照这个说明，如果我想要我的名字的拼音颜色比较特殊，那么定义的子类如下：

```C++
class QscilexerCppAttach : public QsciLexerCPP
{
    Q_OBJECT
public:
    const char *keywords(int set) const;
};

const char * QscilexerCppAttach::keywords(int set) const
{
    if(set == 1 || set == 3)
        return QsciLexerCPP::keywords(set);
    if(set == 2)
        return
        //下面是自定义的想要有特殊颜色的关键字
        "XiaoDong DongDong ";

    return 0;
}
```

这样的类如果加载到对应的`QsciScintilla`即可使用`QsciLexerCPP::KeywordSet2`改变对应自定义的关键字的颜色！

```C++
QsciScintilla *textEdit = new QsciScintilla;
QscilexerCppAttach *textLexer = new QscilexerCppAttach;
textLexer->setColor(QColor(Qt:: yellow),QsciLexerCPP::CommentLine);    //设置自带的注释行为绿色
textLexer->setColor(QColor(Qt::red),QsciLexerCPP::KeywordSet2);   //设置自定义关键字的颜色为红色
textEdit->setLexer(textLexer);
```

这样就可以将`textEdit`控件中自定义关键字“XiaoDong”和“DongDong”显示为红色！好玩吧~

##### 2. 增加关键字自动补全功能

一个好的文本编辑器，一般是可以将常用的字符串自动补全的，这样才让你增加效率，也不用死记硬背一堆关键字，如果使用`QsciScintilla`,这个功能是很好实现的，只要按照以下步骤：

a 定义一个`QsciAPIs`类，构造函数中将父类指向你前面定义的`lexer`类。
b 调用其中的类对应的`add`方法或者`load`方法想要补全的关键字。
c 调用类对应的`prepare`方法来准备好关键字。
d 在你前面定义的`QsciScintilla`类中的相关方法，比如：`setAutoCompletionThreshold`方法用来确定输入几个字符就出现自动补全提示、`setAutoCompletionCaseSensitivity`来说明提示是否大小写敏感之类的。具体可以查看相关文档。

下面还是以`C++`词法分析器来举例子

```C++
QsciScintilla *textEdit = new QsciScintilla;
textLexer = new QscilexerCppAttach;
textLexer->setColor(QColor(Qt:: yellow),QsciLexerCPP::CommentLine);    //设置自带的注释行为绿色
textLexer->setColor(QColor(Qt::red),QsciLexerCPP::KeywordSet2);   //设置自定义关键字的颜色为红色
textEdit->setLexer(textLexer);
//设置自动补全的字符串和补全方式
QsciAPIs *apis = new QsciAPIs(textLexer);
apis->add(QString("move"));
apis->add(QString("moive"));
if(apis->load(QString(":/images/apis")))
    qDebug()<<"读取成功";
else
    qDebug()<<"读取失败";
apis->prepare();
textEdit->setAutoCompletionSource(QsciScintilla::AcsAll);   //自动补全所以地方出现的
textEdit->setAutoCompletionCaseSensitivity(true);   //设置自动补全大小写敏感
textEdit->setAutoCompletionThreshold(1);    //输入一个字符就会出现自动补全的提示
```

倒数第三行的代码这只说明两个注意点：

a `setAutoCompletionSource`可以传入枚举的四种方式，分别代表：

|枚举类型|描述
|:----|:---|
|QsciScintilla::AcsNone|没有自动补全的资源，即禁用自动补全提示功能 
|QsciScintilla::AcsAll|所有可用的资源都要自动补全提示，包括当前文档中出现的名称和使用QsciAPIs类加入的名称
|QsciScintilla::AcsDocument|当前文档中出现的名称都自动补全提示 
|QsciScintilla::AcsAPIs|使用QsciAPIs类加入的名称都自动补全提示 

b `add`方法和`load`方法的区别: `void add(const QString &entry);`通过一个一个调用将需要提示的字符加入，比较麻烦，需要提示的关键字太多的话最好使用`bool load(const QString &filename);`方法，将你需要提示的关键字写到一个文件中，每个关键字都用一行，然后直接`load`方法读取该文件的所有关键字即可。

##### 3. 增加断点调试相关的显示功能

`QsciScintilla`强大的一点是可以增加断点调试相关的显示功能，只要调用`QsciScintilla`相关方法即可，非常简单，具体使用请参考相关文档，我在下面举个例子：

```C++
//行号显示区域
textEdit->setMarginType(0, QsciScintilla::NumberMargin);
textEdit->setMarginLineNumbers(0, true);
textEdit->setMarginWidth(0,30);
//断点设置区域
textEdit->setMarginType(1, QsciScintilla::SymbolMargin);
textEdit->setMarginLineNumbers(1, false);
textEdit->setMarginWidth(1,20);
textEdit->setMarginSensitivity(1, true);    //设置是否可以显示断点
textEdit->setMarginsBackgroundColor(QColor("#bbfaae"));
textEdit->setMarginMarkerMask(1, 0x02);
connect(textEdit, SIGNAL(marginClicked(int, int, Qt::KeyboardModifiers)),this,
        SLOT(on_margin_clicked(int, int, Qt::KeyboardModifiers)));
textEdit->markerDefine(QsciScintilla::Circle, 1);
textEdit->setMarkerBackgroundColor(QColor("#ee1111"), 1);
//单步执行显示区域
textEdit->setMarginType(2, QsciScintilla::SymbolMargin);
textEdit->setMarginLineNumbers(2, false);
textEdit->setMarginWidth(2, 20);
textEdit->setMarginSensitivity(2, false);
textEdit->setMarginMarkerMask(2, 0x04);
textEdit->markerDefine(QsciScintilla::RightArrow, 2);
textEdit->setMarkerBackgroundColor(QColor("#eaf593"), 2);
//自动折叠区域
textEdit->setMarginType(3, QsciScintilla::SymbolMargin);
textEdit->setMarginLineNumbers(3, false);
textEdit->setMarginWidth(3, 15);
textEdit->setMarginSensitivity(3, true);
...
```

这样就可以完成断点提示的相关显示界面啦，总之`QsciScintilla`非常好用，功能非常强大，对于想要实现一个文本编辑器功能的工程师来说，调用这个库绝对能让你省去好多时间！我这里仅仅示例了其冰山一角，能起到抛砖引玉的作用就好啦，聪明的你一点能够用她实现更多更好的功能！

最后，放上本文源码的地址：https://github.com/Dongzhixiao/C-_study_onQT/tree/master/Qscinlla_test


参考链接：

http://pyqt.sourceforge.net/Docs/QScintilla2/index.html
