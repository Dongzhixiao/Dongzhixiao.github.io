---
layout:     post
title:      "余姚的空气真好"
subtitle:   "至少住在这边对身体好！"
date:       2016-08-02
author:     "WangXiaoDong"
header-img: "img/20160802.jpg"
tags:
    - 日记
    - C++
    - Effect C++
---

### 时间:2016年8月2日 天气:晴转小雨:sunny:->:umbrella:
-----
#####   Author:冬之晓:neutral_face:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天，天气终于凉快了一点，到中午的时候还下了一点小雨，真不错。这边比较好
的优点就是天空时刻都是晴朗的，记得当年在西安的时候，天天空气重度污染。但是余
姚这边每天天气都是优！
</pre>

---------

#### 条款03：尽可能使用const

 const的一件奇妙的事情是，它允许你指定一个语义约束，而编译器会强制实施这项约束。它允许你告诉编译器和其他程序员某个数值应该保持不变。只要这(某值保持不变)是事实，你就确实该说出来，因为说出来可以获得编译器的相助，确保这条约束不被违反。
 
- 对于指针来说：**如果关键字const出现在星号左边，表示被指物为常量，如果出现在星号右边，表示指针自身为常量，如果出现在星号两边，表示被指物和指针两者都是常量。**

- 如果被指的类型是常量，则**const出现在类型前、类型后(星号前面)都一样**！

- 对于STL迭代器来说：**STL迭代器前加const，即表示迭代器不得指向不同的东西，但所指的东西的值可以改动，const_iterator表示迭代器所指的东西不可以改动，但迭代器可以指向不同的东西。**例如：  

```C++
std::vector <int> vec;
...
const std::vector<int>::iterator iter =   //iter的作用像个T* const
    vec.begin();
*iter = 10;   //没问题，改变iter所指物
++iter;   //错误！iter是const
std::vector<int>::const_iterator cIter =   //cIter的作用像个const T*
    vec.begin();
*cIter = 10;    //错误！*cIter是const
++cIter;    //没问题，改变cIter
```

##### const可以与函数产生关联，可以用在函数前，函数中（形参表中），函数后。

- 首先考虑，用在函数前，表示返回值是常量。为什么要返回const对象呢？

>我们知道，方法返回值是一个临时对象，只有类型，没有名称，这个临时对象是方法内局部对象的副本。如果返回不是一个常量，客户端就能实现下面的暴行：

```C++
class Rational{...};
const Ration operator* (const Ration & lhs, const Rational & rhs)   //这样定义下面就会报错

Rational a,b,c; 
(a*b)=c; //对方法返回值进行赋值。
```
>对于内置类型，这样的赋值行为是错误的。对于自定义类型，也应该报错。有一个准则要遵守：自定义类型应该和内置类型在行为上保持一致，除非有特殊情况。因此，为了避免客户端对方法返回值进行赋值，请返回一个const对象。

- 然后考虑，const作用在函数中
>const在方法中，也就是出现在形参表中，这种情况很好理解，表示形参不可修改。需要注意的是：形参表不同，可以构成过载。如果形参表相同，只是常量性不同，能否构成过载？能不能构成过载的关键是：编译器能不能根据实参确定调用哪个方法，也就是过载方法的匹配程度不一样。  
如果形参是引用或者指针，可以构成过载，这种情况下，形参是实参的别名，根据实参的常量性，可以确定调用哪个方法。  
如果形参不是引用或者指针，不能构成过载，这种情况下，形参是实参的副本，与实参没有了关系，他们和实参的匹配程度是一样的。

- const在方法后，表示常量方法，应该尽量使用常量方法。
>有两个好处：a、接口容易理解，明确表示不会修改对象内容；b、使得const对象可以调用。考虑，我们知道non-const对象可以调用const成员方法，但是，const对象不能调用non-const成员方法，因为non-const成员方法可能会修改对象。为了让const对象调用non-const成员方法，有两种办法：一是使用const\_cast<T&>或者const\_cast<T*>去除对象的常量性，二是如果不修改对象，使用const成员方法，显然第二种办法更好。  
许多人漠视这样一个事实：**两个成员函数如果常量性不同，可以被重载**。这是一个重要的C\+\+特性。
>>考虑以下class，用来表示一大块文字：

```C++
class TextBlock {
public:
    explicit TextBlock(const std::string& str):text(str){}
    ...
    const char& operator[] (std::size_t position) const
    { return text[position];}
    char& operator[] (std::size_t position) 
    { return text[position];}
private:
    std::string text;
};
```

>>TextBlock的class可以这样使用：

```C++
TextBlock tb("Hello");
std::cout<<tb[0];   //调用non-const TextBlock::operator[]

const TextBlock ctb("World");
std::cout<<ctb[0];   //调用const TextBlock::operator[]
```

>>顺便一提，真实中const对象大多用于passed by pointer-to-const 或者 passed by reference-to-const的传递结果。上述的例子太做作。下面比较常用：

```C++
void print(const TextBlock& ctb)   //此函数中ctb是const
{
    std::cout<<ctb[0];   //调用const TextBlock::operator[]
    ...
}
```

>>只要重载operator[]并对不同的版本给予不同的返回类型，就可以令const和non-const TextBlocks获得不同的处理：那么以下代码错误的原因就是：

```C++
std::cout << tb[0]; //没问题读一个non-const TextBlock  
tb[0] = 'x'; //没问题写一个non-const TextBlock  
std::cout << ctb[0]; //没问题读一个const TextBlock  
ctb[0] = 'x'; //错误！写一个const TextBlcok  
```

>>也请注意：non-const operator[]返回是一个reference-to char，不是char。如果operator[]只返回一个char，下面的句子无法通过编译：  
`tb[0] = 'x';`  
因为，如果函数返回类型是一个内置类型，则改动函数的返回值从来就不合法，纵使合法，C\+\+以by value返回对象这一事实意味着改动其实是tb.text[0]的一个副本，不是其自身，这也不是你想要的结果！

##### 两个流行概念：bitwise constness 和 logical constness

一个更改了“指针所指物”的成员函数虽然不能算是const但只有指针隶属于对象那么函数为bitwise不会引发编译器的异议

```C++
class TextBlock
{
public:
char& operator[](szie_t positions)
{
return text[posiitions];
}
private:
char* ptext;
};
```

这个class不适当地将其operator[] 声明为const成员函数，而该函数却返回一个reference指向对象内部值（条款28对此有深刻讨论）。假设暂时不管这个
事实，请注意，operator[]实现代码并不更改pText。于是编译器很开心地为operator[]产出目标码。它是bitwise const，所有编译器都这么认定。但是看看它允许发生什么事：

```C++
const CTextBlock cctb("Hello");//声明一个常量对象。  
char* pc = &cctb[0];        //调用const operator[]取得一个指针，  
//  指向cctb的数据。  
*pc = 'J';              //cctb现在有了 "Jello" 这样的内容。 
```

这其中当然不该有任何错误：你创建一个常量对象并设以某值，而且只对它调用const成员函数。但你终究还是改变了它的值。
这种情况导出所谓的logical constness。这一派拥护者主张，一个const成员函数可以修改它所处理的对象内的某些bits，但只有在客户端侦测不出的情况下才得如此。例如你的CTextBlock class有可能高速缓存（cache）文本区块的长度以便应付询问：

```C++
class CTextBlock {  
public:  
...  
std::size_t length() const;  
private:  
char* pText;  
std::size_t textLength; //最近一次计算的文本区块长度。  
bool lengthIsValid; //目前的长度是否有效。  
};  
std::size_t CTextBlock::length() const  
{  
if (!lengthIsValid) {  
textLength = std::strlen(pText);//错误！在const成员函数内  
lengthIsValid = true;       //  不能赋值给textLength  
}                           //  和lengthIsValid。  
return textLength;  
} 
```

length的实现当然不是bitwise const，因为textLength和lengthIsValid都可能被修改。这两笔数据被修改对const CTextBlock对象而言虽然可接受，但编译器不同意。它们坚持bitwise constness。怎么办？
解决办法很简单：利用C++(www.cppentry.com) 的一个与const相关的摆动场：mutable（可变的）。mutable释放掉non-static成员变量的bitwise constness约束：

```C++
class CTextBlock {  
public:  
...  
std::size_t length() const;  
private:  
char* pText;  
mutable std::size_t textLength; //这些成员变量可能总是  
mutable bool lengthIsValid;     //会被更改，即使在  
};                              //const成员函数内。  
std::size_t CTextBlock::length() const  
{  
if (!lengthIsValid) {  
textLength = std::strlen(pText);//现在，可以这样，  
lengthIsValid = true;       //也可以这样。  
}  
return textLength;  
} 
```

##### 在const和non-const成员函数中避免重复

对于"bitwise-constness非我所欲"的问题，mutable是个解决办法，但它不能解决所有的const相关难题。举个例子，假设TextBlock（和 CTextBlock）内的operator[] 不单只是返回一个reference指向某字符，也执行边界检验（bounds checking）、志记访问信息（logged access info.）、甚至可能进行数据完善性检验。把所有这些同时放进const和non-const operator[] 中，导致这样的怪物（暂且不管那将会成为一个"长度颇为可议"的隐喻式inline函数--见条款30）：

```C++
class TextBlock {  
public:  
...  
const char& operator[](std::size_t position) const  
{  
...     //边界检验（bounds checking）  
...     //志记数据访问（log access data）  
...     //检验数据完整性（verify data integrity）  
return text[position];  
}  
char& operator[](std::size_t position)  
{  
...     //边界检验（bounds checking）  
...     //志记数据访问（log access data）  
...     //检验数据完整性（verify data integrity）  
return text[position];  
}  
private:  
std::string text;  
}; 
```

哎哟！你能说出其中发生的代码重复以及伴随的编译时间、维护、代码膨胀等令人头痛的问题吗？当然啦，将边界检验……等所有代码移到另一个成员函数（往往是个private）并令两个版本的operator[] 调用它，是可能的，但你还是重复了一些代码，例如函数调用、两次return语句等等。
你真正该做的是实现operator[]的机能一次并使用它两次。也就是说，你必须令其中一个调用另一个。这促使我们将常量性转除（casting away constness）。
就一般守则而言，转型（casting）是一个糟糕的想法，我将贡献一整个条款来谈这码事（条款27），告诉你不要那么做。然而代码重复也不是什么令人愉快的经验。本例中const operator[]完全做掉了non-const版本该做的一切，唯一的不同是其返回类型多了一个const资格修饰。这种情况下如果将返回值的const转除是安全的，因为不论谁调用non-const operator[]都一定首先有个non-const对象，否则就不能够调用non-const函数。所以令non-const operator[]调用其const兄弟是一个避免代码重复的安全做法--即使过程中需要一个转型动作。下面是代码，稍后有更详细的解释：

```C++
class TextBlock {  
public:  
  ...  
  const char& operator[](std::size_t position) const //一如既往  
  {  
    ...  
    ...  
    ...  
    return text[position];  
  }  
  char& operator[](std::size_t position)    //现在只调用const op[]  
  {  
     return  
        const_cast<char&>(          //将op[]返回值的const转除  
          static_cast<const TextBlock&>(*this)//为*this加上const  
              [position]                    //调用const op[]  
        );  
  }  
...  
}; 
```

如你所见，这份代码有两个转型动作，而不是一个。我们打算让non-const operator[]调用其const兄弟，但non-const operator[]内部若只是单纯调用operator[]，会递归调用自己。那会大概……唔……进行一百万次。为了避免无穷递归，我们必须明确指出调用的是const operator[]，但C\+\+缺乏直接的语法可以那么做。因此这里将 \*this从其原始类型TextBlock& 转型为const TextBlock\&。是的，我们使用转型操作为它加上const！所以这里共有两次转型：第一次用来为 \*this添加const（这使接下来调用operator\[\]时得以调用const版本），第二次则是从const operator\[\]的返回值中移除const。
添加const的那一次转型强迫进行了一次安全转型（将non-const对象转为const对象），所以我们使用static\_cast。移除const的那个动作只可以藉由const\_cast完成，没有其他选择（就技术而言其实是有的；一个 C-style转型也行得通，但一如我在条款27所说，那种转型很少是正确的抉择。如果你不熟悉static\_cast或const\_cast，条款27提供了一份概要）。
至于其他动作，由于本例调用的是操作符，所以语法有一点点奇特，恐怕无法赢得选美大赛，但却有我们渴望的"避免代码重复"效果，因为它运用const operator\[\] 实现出non-const版本。为了到达那个目标而写出如此难看的语法是否值得，只有你能决定，但"运用const成员函数实现出其non-const孪生兄弟"的技术是值得了解的。
更值得了解的是，反向做法--令const版本调用non-const版本以避免重复--并不是你该做的事。记住，const成员函数承诺绝不改变其对象的逻辑状态（logical state），non-const成员函数却没有这般承诺。如果在const函数内调用non-const函数，就是冒了这样的风险：你曾经承诺不改动的那个对象被改动了。这就是为什么"const成员函数调用non-const成员函数"是一种错误行为：因为对象有可能因此被改动。实际上若要令这样的代码通过编译，你必须使用一个const\_cast将\*this身上的const性质解放掉，这是乌云罩顶的清晰前兆。反向调用（也就是我们先前使用的那个）才是安全的：non-const成员函数本来就可以对其对象做任何动作，所以在其中调用一个 const成员函数并不会带来风险。这就是为什么本例以static\_cast作用于*this 的原因：这里并不存在const相关危险。
本条款一开始就提醒你，const是个奇妙且非比寻常的东西。在指针和迭代器身上；在指针、迭代器及references指涉的对象身上；在函数参数和返回类型身上；在local变量身上；在成员函数身上，林林总总不一而足。const是个威力强大的助手。尽可能使用它。你会对你的作为感到高兴。

#### 请记住：

- [x] ***将某些东西声明为const可帮助编译器侦测出错误用法。const可被施加于任何作用域内的对象、函数参数、函数返回类型、成员函数本体。***
- [x] ***const成员函数意味着强制实施bitwise constness,，即不可以更改对象任何non-static数据成员，因为const修饰的是this所指的对象，而不是static数据成员，但编写程序时可以使用“概念上的常量性”（conceptual constness)，只要在客户端侦测不出的情况下。实施"概念上的常量性“有可能需要在const成员函数中修改对象的内容，此时可以使用关键字mutable，mutable可以释放non-static数据成员的bitwise constness约束。***
- [x] ***当const和non-const成员函数有着实质等价的实现时，令non-const版本调用const版本可避免代码重复，但反向做法，令const版本调用non-const版本行不通。因为const成员函数意味着不改变其对象的逻辑状态，如果const成员函数调用non-const成员函数，则有可能改变其对象的逻辑状态。而non-const成员函数本来就可以对其对象做任何动作，所以在其中调用const成员函数并不会带来风险。***