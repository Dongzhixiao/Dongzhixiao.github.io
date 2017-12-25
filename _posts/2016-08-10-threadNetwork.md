---
layout:     post
title:      "阅读代码"
subtitle:   "2016/08/10 发现不足，多线程，网络通信"
date:       2016-08-10
author:     "WangXiaoDong"
header-img: "img/20160810.jpg"
tags:
    - 日记
    - 文档生成工具
    - Doxygen
---

### 时间:2016年8月10日 天气:晴:sunny:
-----
#####   Author:冬之晓:dizzy_face:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天阅读代码，关于UDP和TCP通信和多线程那一块，以前一直感觉这部分内容比较难，但是今天一看，
    其实在Qt的封装下，一点也不难，只要继承对应的QTcpSocket、QUdpSocket和QThread，
    就可以很方便的使用网络编程和多线程编程啦！
</pre>

- @headerfile <header-file> [<header-name>]

用于类，结构，联合的文档，同时这些文档都位于它们定义的前面。此命令的参数与@cmdclass命令的第二、三个参数相同。`header-file`名称会引用一个文件，它能被应用程序为获取类，结构，联合的定义而包含。`<header-name>`参数可用于覆盖`<header-file>`之外的类文档中所使用链接名称，如果在默认包含路径中无法定位包含文件(如<X11/X.h>)时，`<header-name>`就非常有用。

你也可以指定`<header-name>`，使用引号或尖括号中加入头名，使之看起来象是包容语句。尖括号也可用于已给出的头名。若`<header-file>和<header-name>`都使用双引号，当前文件(该命令查到的那个文件)将被引用。为了将一个`@headerfile`命令的注释块插入到`test.h`文件中，以下有三种等价的命令操控方式：

```C++
@headerfile test.h "test.h"
@headerfile test.h ""
@headerfile "" 
```

使用尖括号时你无需指定目标，如果你希望明确地指出，也可以使用一下方式：

```C++
@headerfile test.h <test.h>
@headerfile test.h <>
@headerfile <> 
```

- @hideinitializer

一个定义的缺省值和一个变量的初始化都将显示在文档中，除非它们的行数超过30。放置该命令在一个定义或者变量的注释块中，初始化将一直被隐藏。

- @idlexcept <name>

表明了该注释块包含的文档是为了一个名字为`<name>`的IDL异常

- @implements <name>

用于手动指定一个继承关系，当编程语言不支持这个特性时(如C语言)。

- @ingroup (<groupname> [<groupname> <groupname>])

若该命令被放置到一个类，文件，命名空间的注释块中，它将添加一个`<groupname>`的组或者是组id。

- @interface <name> [<header-file>] [<header-name>]

为一个名字为`<name>`的接口指定一个文档注释块，参数与`@class`命令相同

- @internal  

该命令开始一个仅仅为了内部使用的文档段，这个段落在注释块结尾处结束。你也可以通过使用命令`@endinternal`提前结束该内部文档段。

如果该命令被放置的一个小节中(见@section中的例子)，那么命令后的所有子小节将被看成internal，只有与其在同一层级的新节才能重新可见。

- @mainpage [(title)]

如果该命令被放置到一个注释块，此块将被用于定制HTML的索引页，或latex的首章。这个很常用，就是生成段落的开始页面的内容，我一开始研究了好久才在stackoverflow上面查到应该这样用！！

title参数是可选的，并能替换掉doxygen生成的默认标题。如果你不需要任何标题，可以在命令后指定notitle。
例如：

```C++
/*! \mainpage My Personal Index Page
 *
 * \section intro_sec Introduction
 *
 * This is the introduction.
 *
 * \section install_sec Installation
 *
 * \subsection step1 Step 1: Opening the box
 *
 * etc...
 */
```

- @memberof <name>

该命令使得类中的一个成员函数与@relates有相同的功能。 只有用这个命令函数才能被表述为一个类的真正成员。当编程语言不支持成员函数的概念时它将非常有用(如C语言)。

- @name [(header)]

该命令将成员组中的头定义转变成一个注释块，注释块将位于一个包含成员组的`//@{ ... //@}` 块。

- @namespace <name>

为一个名字为`<name>`的命名空间指定一个文档注释块

- @nosubgrouping

可放置在一个类文档中，用于在合并成员组时，避免doxygen放置一个公有，保护或者私有的子分组。

- @overload [(function declaration)]

用于为一个重载成员函数生成后续的标准文本：这是一个未来便利而提供的重载函数，它不同于上述函数，只包含那些它接受的参数。

如果在函数声明或者定义之前，无法定位重载成员函数的文档，能用可选参数来指定正确的函数。在生成的消息之后将会附带文档块内其他的文档。

注意：你要承担起确认文档的重载成员正确与否的责任，在这种情况下为了防止文档重新排列，必须将`SORT_MEMBER_DOCS`设置为NO。

注意：在一行注释中@overload命令无法工作

例如：

```C++
class Overload_Test 
{
public: 
    void drawRect(int,int,int,int); 
    void drawRect(const Rect &r);
};
void Overload_Test::drawRect(int x,int y,int w,int h) {}
void Overload_Test::drawRect(const Rect &r) {}
/*! @class Overload_Test 
 *  @brief A short description. 
 *
 *  More text. 
 */
/*! @fn void Overload_Test::drawRect(int x,int y,int w,int h) 
 * This command draws a rectangle with a left upper corner at ( \a x , \a y ), 
 * width \a w and height \a h.  
 */
/*!
 * @overload void Overload_Test::drawRect(const Rect &r) 
 */
```

- @package <name>

为一个名字为`<name>`的Java包指定一个文档注释块。

- @page <name> (title)

指定一个注释块，它包含了一个与特定的类，文件，成员非直接关联的文档块。HTML生成器将创建一个包含此文档的页面，而latex生成器将在'页文档'的章节中开始一个新的小节。

例如：

```C++
/*! @page page1 A documentation page  
    @tableofcontents  Leading text.  
    @section sec An example section  This page contains the subsections @ref subsection1 and @ref subsection2.  
    For more info see page @ref page2.  
    @subsection subsection1 The first subsection  Text.  
    @subsection subsection2 The second subsection  
    More text.
 */
/*! @page page2 Another page  
    Even more info.
*/
```

- @private

指定注释块中的私有文档化成员，它只能被同一类的其他成员访问。

注意：doxygen会自动确认面对对象语言中的成员保护层级，此命令用于不支持保护层级的概念的语言。(如C语言和PHP4)

私有成员小节的开始处，有一种与C++中`private: `类似的标记法，使用@privatesection

- @privatesection

开始一个私有成员的小节，与C++中private类似的标记法，指定注释块中的私有文档化成员，它只能被同一类的其他成员访问。

- @property (qualified property name)

为一个属性(即可是全局也可是一个类成员)指定一个文档注释块，此命令等价于@var @fn

- @protected

指定注释文档中的保护文档化成员，它只能被同一类的其他成员或派生类访问。

注意：doxygen会自动确认面对对象语言中的成员保护层级，此命令用于不支持保护层级的概念的语言。(如C语言和PHP4)

保护成员开始处，有一种与C++中`protected: `类似的标记法，使用@privatesection

- @protocol <name> [<header-file>] [<header-name>]

为Object-C中名字为`<name>`的协议指定一个文档注释块，它的参数与`@class`相同。

- @public

指定注释文档中的公有文档化成员，它只能其他类或函数访问。

注意：doxygen会自动确认面对对象语言中的公有层级，此命令用于不支持公有层级的概念的语言。(如C语言和PHP4)

公有成员开始处，有一种与C++中`public: `类似的标记法，使用@publicsection

- @pure

指定注释文档中纯虚成员，它是抽象的没有被实施的成员。

注意：此命令用于不支纯虚方法的概念的语言。(如C语言和PHP4)

- @relates <name>

用于名字为`<name>`非成员函数的文档，它可以放置在这个函数到类文档的“关联函数”小节内，该命令对于非友元函数与类之间建立强耦合是非常有用的。它无法用于文件，只能用于函数。

```C++
/*!  
 * A String class. 
 */
class String
{ 
    friend int strcmp(const String &,const String &);
};
/*! 
 * Compares two strings. 
 */
int strcmp(const String &s1,const String &s2)
{
}
/*! @relates String 
 * A string debug function. 
 */
void stringDebug() 
{
}
```

- @related <name>

同@relates

- @relatesalso <name>

用于名字为`<name>`非成员函数的文档，它可以放置在这个函数到类文档的“关联函数”小节内，也可是远离它正常文档的位置。该命令对于非友元函数与类之间建立强耦合是非常有用的。只能用于函数。

- @showinitializer

只显示定义的默认值和变量的初始化，如果它们的文本行数小于30，在变量或定义的注释块中放置这个命令，初始化过程将无条件被显示出来。

- @static

指定注释文档中静态成员，它定义在类中，但不属于类的实例化，它属于整个类！

注意：此命令用于不支静态方法的概念的语言。(如C语言和PHP4)

- @struct <name> [<header-file>] [<header-name>]

为一个名字为`<name>`的结构体指定一个文档注释块，它与参数@class相同

- @typedef (typedef declaration)

为一个类型转换指定一个文档注释块，它与参数@var @fn相同

- @union <name> [<header-file>] [<header-name>]

为一个名字为`<name>`的联合体指定一个文档注释块，它与参数@class相同

- @var (variable declaration)

为一个变量或者枚举值指定一个文档注释块，它与参数@typedef @fn相同

- @vhdlflow [(title for the flow chart)]

这是一个硬件描述语言特定的命令，可以放在文档中产生一个逻辑过程的流程图。流程图的标题可以随意指定。

- @weakgroup <name> [(title)]

用法与@addtogroup相似，但当解决组定义冲突时，它将会有一个更低的优先级。



今天就到这里吧，明天开始**小节指示命令**