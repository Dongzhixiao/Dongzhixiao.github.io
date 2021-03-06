---
layout:     post
title:      "程序外包的缺点"
subtitle:   "2016/08/29 发现接手外包程序真的是太多bug了"
date:       2016-08-29
author:     "WangXiaoDong"
header-img: "img/20160829.jpg"
tags:
    - 日记
    - Qt
    - Effect C++
---

### 时间:2016年8月29日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    我发现自从接到这个所谓的“外包”程序后，各种bug接踵而至。每天我都在和bug打交道，在不断地思考中发现问题的解决方案。
	为了让我下载遇到同样的问题时不至于重视重复无用功，今天要把今天遇到的问题记录下来，以防忘记！
</pre>

以前，我在使用C\+\+编程的时候，总是忽略强制类型转换和安全的转换之间的区别，直到今天遇到这个问题，让我以后绝对要使用安全的转换方式，否则实在太坑了！

今天遇到的问题是在实现一个动态界面的时候出现的。在使用QTreeWidget时，我需要根据一些数据设置每一个QTreeWidgetItem里面包含的QComboBox中的项目类型和数量。然后我需要知道当前选择的QComboBox到底属于哪个QTreeWidgetItem，在这种情况下我可以通过Qt专有的函数得到当前选择的控件，并转换为QComboBox，然后操控这个控件：

```C++
QWidget * curSelect = QApplication::focusWidget();  //得到当前使用的控件
QComboBox * curCom = (QComboBox *)curSelect;

if(curCom != nullptr)
{
    ....   //使用这个控件的代码
}
```

然后我再测试的时候，发现就是我当前选择的不是QComboBox，也能够进入if里面的语句！结果就会偶尔出现错误。最后，我明白了，代码中的第二句话的强制转换无论curSelect是什么控件，都能够转换成功，如果想要当curSelect不属于QComboBox的类型或者其子类的时候不能转换，应该修改为以下形式：

```C++
QWidget * curSelect = QApplication::focusWidget();  //得到当前使用的控件
QComboBox * curCom = qobject_cast<QComboBox *>(curSelect);

if(curCom != nullptr)
{
    ....   //使用这个控件的代码
}
```

这样子的话如果想要当curSelect不属于QComboBox的类型或者其子类的时候就会使得curCom = nullptr，即为空！！

强制类型转换如果进行地址的转换时，无论转换后截断也好，超位也好，都能够成功。这样是非常不安全的！！


**下面再来学习一节《Effective C++》**

#### 条款15：在资源管理中提供对原始资源的访问

下面代码：

```C++
tr1::shared_ptr<Investment> pInv(createInvestment());  
int daysHeld(const Investment* pi);  
```

我们要调用daysHeld函数的话，就必须传递一个Investment指针，但是我们现在只有pInv对象，所以我们需要一个函数可将RAII class（本例为tr1::shared_ptr）对象转换为其所内含之原始资源（本例）。
有两种方法，一种是显式转换，另外一种是隐式转换。

 

- 显式转换

tr1::shared\_ptr和auto\_ptr都提供一个get成员函数，用来执行显式转换，也就是它会返回智能指针内部的原始指针（的复件）：

```C++
int days = daysHeld(pInv.get()); //将pInv内的原始指针传给daysHeld  
```


tr1::shared_ptr和auto_ptr重载了指针取值操作符（operator->和operator*），这样的操作的话，它们允许隐式转换至底部原始指针。看下面的示例：

```C++
class Investment {  
public:  
    bool isTaxFree() const;  
};  
  
Investment* createInvestment();  
tr1::shared_ptr<Investment> pi1(createInvestment());  
  
bool taxable1 = !(pi1->isTaxFree());   //经由operator->访问资源  
  
auto_ptr<Investment> pi2(createInvestment());  
bool taxable2 = !((*pi2).isTaxFree());  //经由operator*访问资源  
```

因为有隐式转换，所以pi1.operator->()  与 pi2.operator*() 返回的是底部原始指针与底部与原始对象！！

 

- 隐式转换

先回顾下上文提到的显示转换：

```C++
FontHandle getFont();  
void releaseFont(FontHandle fh);  
  
class Font {  
public:  
    explicit Font(FontHandle fh) : f(fh)   
    { }  
    ~Font() {  
        releaseFont(f);  
    }  
    FontHandle get() const {  
        return f;  
    }  
private:  
    FontHandle f;  
};  
```

上面这个Font类提供了一个返回底部资源的函数get()，这看起来很beutiful。。

但是这使得客户每当想要使用API时就必须调用get() 看下面的代码：

```C++
void changeFontSize(FontHandle f, int newSize);  
Font f(getFont());  
int newFontSize;  
...  
changeFontSize(f.get(), newFontSize);  
```

有些程序员认为这样每次都要调用get函数，太麻烦了！所以他们想出了隐式转换！

代码如下：

```C++
class Font {  
public:  
    explicit Font(FontHandle fh) : f(fh)   
    { }  
    ~Font() {  
        releaseFont(f);  
    }  
    operator FontHandle() const {    //隐式转换函数  
        return f;  
    }  
private:  
    FontHandle f;  
};  
```

上面这个class提供了一个隐式转换函数，所以我们可以像下面这样用它：

```C++
void changeFontSize(FontHandle f, int newSize);  
Font f(getFont());  
int newFontSize;  
...  
changeFontSize(f, newFontSize);  
```

但是这样的隐式转换会增加错误发生的机会，例如我们可能会在需要Font时意外创建一个FontHandle：

```C++
Font f1(getFont());  
FontHandle f2 = f1;  //原意是要拷贝一个Font对象，但是却误将f1转换为了FontHandle对象。  
```

这样的话，f1就被转化为了FontHandle对象，很容易发生错误！因为，当f1被销毁的时候，f2就因此而成为“虚吊的”。


具体用显式转换还是隐式转换视情况而定，答案主要取决于RAII class被设计执行的特定工作，以及它被使用的情况。

最佳的设计是条款18：让接口容易被正确使用，不易被误用。

通常显式转换get比较受欢迎，因为它减少了出错的可能性。然而有时候，隐式转换所带来的“自然用法”也会印发天平倾斜。

请记住：

- [x] ***APIs往往要求访问原始资源，所以每一个RAII class应该提供一个“取得其所管理之资源”的办法。***

- [x] ***对原始资源的访问可能经由显式转换或隐式转换。一般而言显式转换比较安全，但隐式转换对客户比较方便。***
