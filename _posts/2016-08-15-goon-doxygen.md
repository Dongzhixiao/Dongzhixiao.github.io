---
layout:     post
title:      "孤单"
subtitle:   "2016/08/15 继续Doxygen学习"
date:       2016-08-15
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160815.jpg?raw=true"
tags:
    - 日记
    - Doxygen
---

### 时间:2016年8月15日 天气:晴:sunny:
-----
#####   Author:冬之晓:dizzy_face:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天，江威去上海面试了，无论如何，现在余姚只剩下我一个人了有点孤单，特别是想到远方的家人，
	所以今天我申请了单人宿舍，以后家里来人了还可以住！
</pre>

今天继续`doxygen`的学习

#### 创建连接命令

- @addindex (text)

添加1文本到latex索引中

- @anchor <word>

放置一个不可见的已命名的anchor(固定点)到文档中，且能使用`@ref`命令进行引用。
注意：anchor目前只能放置在一个标记成页面(使用`@page`)或主页(使用`@mainpage`)的注释块中

- @cite <label>

增加一个书籍参考到一个文本和数目参考列表里。

- @endlink

终止一个命令`@link`开始的连接

- @link <link-object>

连接是由doxygen自动生成的，且一直有一个指向的对象名作为连接文本。该命令可以用于创建一个对象(一个文件，类或者成员)的连接，并且用户可指定对象的连接文本。此命令可以被`@endlink`终止，所有在其之间的文本都将作为指向`@link`第一个参数`<link-object>`的连接文本。

- @ref <name> ["(text)"]

创建一个已命名小节，子小节，页面或anchor(固定点)的引用，在HTML文档中该命令将生成一个指向小节的连接。在一个小节或者子小节中将使用小节标题作为连接文本，在anchor(固定点)中可使用引号间的text或者当text忽略时使用`<name>`作为连接文本，而在latex文档中，如果`<name>`引用的是一个anchor(固定点)，该命令将小节生成一个编号或者是使用一个页面编号后的文本。

- @refitem <name>

就像`@ref`命令，这个命令创建一个名字小节的引用，但是这个在一个以`@secreflist`开始，` @endsecreflist`结束的列表中。

- @secreflist

开始一个标题列表，用`@refitem `创建，每一个都连接到对应的名字小节

- @endsecreflist

开始以`@secreflist`开头的标题列表。

- @subpage <name> ["(text)"]

创建若干页面的一个层次，相同结构也可以使用`@defgroup,@ingroup`命令，但在页面中使用`@subpage`命令更方便。主页`@mainpage`通常是层次的根节点。

该命令类似于`@ref`，它用于创建一个到`<name>`页面标签的引用，并且可选择第二个参数指定的text作为连接文本。

它不同于`@ref`命令之处在于，它只能工作在页面中，在页面中创建一个父子关联，并且子页面使用`<name>`进行标识。

例如：

```C++
/*! @mainpage A simple manual

Some general info.

This manual is divided in the following sections:
- @subpage intro
- @subpage advanced "Advanced usage"
*/

//-----------------------------------------------------------

/*! @page intro Introduction
This page introduces the user to the topic.
Now you can proceed to the @ref advanced "advanced section".
*/

//-----------------------------------------------------------

/*! @page advanced Advanced Usage
This page is for advanced users.
Make sure you have first read @ref intro "the introduction".
*/
```

- @tableofcontents

在页面顶端创建一个内容表格，列出页面中所有小节和子小节

警告：该命令只能工作在一个相关页文档块的小节中，而无法工作于其他文档块中而且仅仅在HTML输出中有效。

- @section <section-name> (section title)

创建一个名为`<section-name>`的小节，小节的标题可使用命令的第二参数指定。

警告：该命令只能工作在一个相关页文档块的小节中，而无法工作于其他文档块中。

- @subsection <subsection-name> (subsection title)

创建一个名为`<section-name>`的子小节，小节的标题可使用命令的第二参数指定。

警告：该命令只能工作在一个相关页文档块的小节中，而无法工作于其他文档块中。

- @subsubsection <subsubsection-name> (subsubsection title)

创建一个名为`<section-name>`的子小节的小节，小节的标题可使用命令的第二参数指定。

警告：该命令只能工作在一个相关页文档块的小节中，而无法工作于其他文档块中。

- @paragraph <paragraph-name> (paragraph title)

创建一个名为`<paragraph-name>`的段落，它的标题可使用命令的第二参数指定。

警告：该命令只能工作在一个相关页文档块的小节中，而无法工作于其他文档块中。



