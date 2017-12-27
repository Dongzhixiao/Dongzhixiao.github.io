---
layout:     post
title:      "接到猎头电话"
subtitle:   "2016/08/25 第一次接到猎头电话，发现自身能力不足"
date:       2016-08-25
author:     "WangXiaoDong"
header-img: "img/20160825.jpg"
tags:
    - 日记
    - Effect C++
---

时间:2016年8月25日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天,接到了猎头的电话，这是我第一次接到猎头的电话，对方给我推荐了C\+\+的岗位，是一家外企。
	但是最后我说不行，因为这家公司需要流利的英语，而我的口语实在是不行，因此面试肯定无法通过。
	哎，英语口语不好真的是对找工作非常不利！
</pre>

#### 条款11：在 operator= 中处理“自我赋值”

“自我赋值”发生在对象被赋值给自己时:

```C++
class Widget
{
    ...
};
 
Widget w;
...
w = w;　　//赋值给自己
```

自我赋值，看起来愚蠢，但是却合法。所有不用认定客户不会这么做。此外赋值动作并不总是那么可被一眼辨别出来，例如：

`a[i]= a[j];   //潜在的自我赋值`

如果i和j有相同的值，这便是自我赋值。再看：

`*px = *py;   //潜在的自我赋值`

如果px和py恰巧指向同一个东西，这也是自我赋值。这些并不明显的自我赋值，是“别名”带来的结果：

operator=，不仅不具备“自我赋值安全性”，也不具备“异常安全性”。

让operator= 具备“异常安全性”往往自动获得“自我赋值安全性”的回报。因此越来越多的人对“自我赋值”的处理态度是不去管它，而把焦点放在实现“异常安全性”上。

确保代码不但“异常安全”而且“自我赋值安全”的一个替代方案是，使用所谓的copy and swap技术。此技术和“异常安全性”有密切关系，它是一个常见而够好的operator=撰写办法，其实现方式为：

```C++
class Widget
{
public:　　　　...
    void swap(Widget& rhs);     //交换*this和任rhs的数据
    ...
};
 
 Widget& Widget::operator=(const Widget& rhs)
 {
     Widget temp(rhs);  //为rhs数据制作一份复件（副本）
     swap(temp);        //将*this数据和上述复件的数据交换
     return *this;
 }
 ```
 
 另外一种实现方式为：
 
 ```C++
 Widget& Widget::operator=(Widget rhs)  //rhs是被传对象的一份复件（副本），注意此处是值传递 pass by value
{
    swap(rhs);     //将*this数据和复件的数据交换
    return *this;
}
```

上述实现方式因为：1、某类的copy assignment操作符可能被声明为“以by value方式接受实参”；2、以by value方式传递东西会造成一份复件/副本

此方式牺牲了清晰性，然而将拷贝动作从函数本体移至“函数参数构造阶段”却可令编译器有时生产更高效的代码

请牢记：

- [x] ***确保当前对象自我赋值时operator= 有良好行为。其中技术包括比较“来源对象”和“目标对象”的地址、精心周到的语句顺心、以及copy-and-swap。***

- [x] ***确定任何函数如果操作一个以上的对象，而其中多个对象是同一个对象时，其行为仍然正确。***