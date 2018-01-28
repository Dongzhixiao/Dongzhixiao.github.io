---
layout:     post
title:      "发现新书"
subtitle:   "2016/07/31 好多知识自己都没有学过"
date:       2016-07-31
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160731.jpg?raw=true"
tags:
    - 日记
    - C++
    - Effect C++
---


### 时间:2016年7月31日 天气:晴:sunny:
-----
#####   Author:冬之晓:neutral_face:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天是周日，因此起来的比较晚，差不多8点半才起床，感觉有点内疚，明明自己还
有很多知识没有学习，可是还会起床比较晚。我必须好好计划一下自己以后的生活，否则
肯定无法将程序学到极致。今天看了看《C++ GUI Qt 
4编程(第二版)》，发现里面好多知识自己都没有学过，当年我在做界面程序的时候，有
好多问题竟然都能在这本书了找到答案！自己真的是太笨了，如果早一点看这本书也不会
在当时编程序的时候浪费这么多的时间！！晚上，江威搬到我们宿舍了。希望明天他能够
顺利入职。
</pre>
--------
##### 命名习惯

- `lhs(left-hand side)`和`rhs(right-hand side)`通常作为二元操作符函数的参数名称：
`const Rational operator* (const Rational &lhs,const Rational &rhs);`

- **指向一个T型对象**命名为pt，即`pointer to T`：

```C++
Widget *pw;   //pw = "pointer to Widget"

class Airplane;
Airplane *pa;   //pa = "pointer to Airplane"

class GameCharacter;
GameCharacter *pgc;   //pgc = "pointer to GameCharacter"
```

- **指向一个T型应用**命名为rt，即`reference to T`：

```C++
Widget &rw;   //rw = "reference to Widget"

class Airplane;
Airplane &ra;   //ra = "reference to Airplane"

class GameCharacter;
GameCharacter &rgc;   //rgc = "reference to GameCharacter"
```
-----------
##### 条款1 视C\+\+为一个语言联邦

C\+\+是一个多重泛型编程语言同时支持：面向过程形式、面向对象形式、函数形式、泛型形式、元编程形式。  
为了理解C\+\+，你必须认识主要的四个次语言：

- **C**。.区块(blocks)，语句(statements)，预处理器(proprocessor)，内置数据类型(built-in data types)，数组(arrays)，指针(pointers)等统统来自C，许多C++对问题的解法其实不过是较高级的C解法。
- **面向对象的C++**。这部分是“C with Classes”所诉求的：classes(包括构造函数和析构函数)，封装(encapsulation)，继承(inheritance)，多态(polymorphism)，虚函数(动态绑定)...等等，这一部分是面向对象在C++上的直接实施.。
- **泛型C++**。这是C++的泛型编程(generic programming)部分，由于templates威力强大，它们带来崭新的变成范型(programming paradigm)，也就是所谓的template meta-programmingTMP，模板元编程)。
- **STL库**。它包括容器(containers)，迭代器(iterators)，算法(algorithms)以及函数对象(function objects)。

当从某个次语言切换到另一个时，高效编程守则可能要改变策略，例如对于内置类型(C-like)而言pass-by-value比pass-by-reference要高效(pass-by-value更占内存，而pass-by-value经过了对指针的一层封装更占时间)，但当从C part of C++迁往Obiect-Oriented C++时，由于用户自定义构造函数和析构函数的存在，pass-by-reference显然比pass-by-value效率更高，运用template时尤其如此，因为所处理对象的类型甚至不都不确定，然而一旦跨入STL，对于迭代器和函数对象而言，旧式的C pass-by-value守则再次适用(因为迭代器和函数对象都是在C指针之上塑造出来的)。

综上所述，C\+\+由四个次语言组成，每个次语言都有自己的规约，使用不同的次语言时C\+\+的高效编程守则也发生变化。
 
#### 请记住：
- [x] ***高效编程编程守则视状况而变化，取决于你使用C\+\+的哪一个部分。***
