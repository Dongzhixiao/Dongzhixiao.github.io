---
layout:     post
title:      "多人开发的问题"
subtitle:   "2016/08/24 多人开发时他人可能产生的bug"
date:       2016-08-24
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160824.jpg?raw=true"
tags:
    - 日记
    - Effect C++
---

### 时间:2016年8月24日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天在调试程序的时候，发现程序中还是有很多bug，关键这些bug是另一位同事产生的，所以我非常无奈，但是就我一个人做这一块。
	而另一个同事是兼职的，因此只能我一个人仔细研究了，加油！！！
</pre>

#### 条款10：令 operator= 返回一个 reference to *this

关于赋值，有趣的是你可以把它们写成连锁形式：

```C++
int x, y, z;
x = y = z = 15;   
```
 
同样有趣的是，赋值采用右结合律，所以上述连锁赋值被解析为：

`x = (y = (z = 15));`

这里15先被赋值给z，然后其结果(更新后的z)再被赋值给y，然后其结果(更新后的y)再被赋值给x。

为了实现“连锁赋值”，赋值操作符必须返回一个reference指向操作符的左侧实参。这是你为classes实现赋值操作符时应该遵循的协议：
     
```C++
class Widget {
public:
  ...
  Widget& operator=(const Widget& rhs)  //返回类型是个reference，指向当前对象。
  {
    ...
    return* this;                       //返回左侧对象
  }
  ...
};
```

这个协议不仅适用于以上的标准赋值形式，也适用于所有赋值相关运算，例如：

```C++
class Widget {
public:
  ...
  Widget& operator+=(const Widget& rhs)  //这个协议适用于+=，-=，*=，等等。
  {
    ...
    return *this;
  }
  Widget& operator=(int rhs)             //此函数也适用，即使此一操作符的参数类型不符协议。
  {
    ...
    return *this;
  }
};
```

注意，这只是个协议，并无强制性。如果不遵循它，代码一样可通过编译。然而这份协议被所有内置类型和标准程序库提供的类型如： string， vector， complex， tr1::shared_ptr或即将提供的类型(见条款54)共同遵守。因此除非你有一个标新立异的好理由，不然还是随众吧。

请记住：

- [x] ***令赋值操作符返回一个reference to *this。 ***