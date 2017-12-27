---
layout:     post
title:      "工作变化"
subtitle:   "2016/08/16 开始汇报每天完成任务和第二天计划"
date:       2016-08-16
author:     "WangXiaoDong"
header-img: "img/20160816.jpg"
tags:
    - 日记
    - Doxygen
---

### 时间:2016年8月16日 天气:晴:sunny:
-----
#####   Author:冬之晓:dizzy_face:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<per>
    今天开始，老板要我们每天汇报完成的任务和第二天的计划，以后要累啦！
</pre>

#### 用于显示例子(将被插入文档中的代码)的命令

- @dontinclude <file-name>

用于解析一个源文件，且不论是否被文档完整包含(如同`@include`命令所做的)，如果你希望将源文件分割成最小的块，并在这些块中间添加文档，那此命令会非常有用。源文件或是目录可使用配置文件中的`EXAMPLE_PATH`标记来指定。

在解析包含`@dotinclude`命令的注释块期间，代码段中类和成员的声明和定义将被'记忆'。

对于单行可使用源文件的单行描述，而显示一行或多行例子可使用`@line,@skip,@skipline,@until`命令，为了这些命令将会使用一个内部指针，`@dontinclude`可设定这个指针指向例子的第一行。

```C++
/*! A test class. */
class Include_Test
{
public: 
    /// a member function
    void example();
};
/*! @page example 
 *  @dontinclude include_test.cpp 
 *  Our main function starts like this: 
 *  @skip main 
 *  @until { 
 *  First we create an object @c t of the Include_Test class. 
 *  @skipline Include_Test 
 *  Then we call the example member function  
 *  \line example
 *  After that our little test routine ends.
 *  \line }
 */
```

例子文件example_test.cpp如下：

```C++
void main()
{
    Example_Test t;  
    t.example();
}
```


- @include <file-name>

用于包含一个源文件作为一个代码块，此命令带有一个包含文件名的参数，源文件或是目录可使用配置文件中的`EXAMPLE_PATH`标记来指定。

如果在`EXAMPLE_PATH`标记中指定的`<file-name>`此例子文件的设定，并非是唯一的，就必须包含`<file-name>`的绝对路径以消除二义性。

使用`@include`命令等价于在文档中插入文件，并使用`@code,@endcode`命令限定文件的范围。

`@include`命令的主要目的是，避免在包含多个源文件和头文件的例子块中出现代码重复。

源文件的单行描述，可使用组合了`@line,@skip,@skipline,@until`命令的`@dotinclude`命令。

注意：doxygen特殊命令是无法在代码块中工作的，但允许在代码块中放置嵌套的C风格注释。也可查看`@example,@dontinclude,@verbatim`命令。

- @includelineno <file-name>

具备与`@include`命令相同的工作模式，但可添加行号到包含文件中。

- @line ( pattern )

用于搜索行，除非该命令找到一个非空行，否则将搜索至最后用`@include`或`@dontinclude`包含的例子，如果该行包含指定的模式，将写入输出。

在例子中会使用一个跟踪当前行的内部指针，将找到非空行作为起始行，并设定内部指针。(如果未找到符合要求的行，指针也将指向例子的末端)

- @skip ( pattern )

用于搜索行，除非该命令找到一个包含指定模式的行否则将搜索至最后用`@include`或`@dontinclude`包含的例子。

在例子中会使用一个跟踪当前行的内部指针，将找到那行作为起始行，并设定内部指针。(如果未找到符合要求的模式，指针也将指向例子的末端)

- @skipline ( pattern )

用于搜索行，除非该命令找到一个包含指定模式的行，否则将搜索至最后用`@include`或`@dontinclude`包含的例子,并将此行写入输出。

在例子中会使用一个跟踪当前行的内部指针，将写入输出的哪行作为起始行，并设定内部指针。(如果未找到符合要求的模式，指针也将指向例子的末端)

- @snippet <file-name> ( block_id )

不像`@include`命令可以用来包含一个完整的文件作为源代码，这个命令中用来标记一个段落作为原文件。如果这是用作`<file-name>`当前文件作为文件的片段。

例如,下面的命令在文档中,引用一个片段文件的例子。cpp驻留在一个子目录应由EXAMPLE_PATH指出。

`\snippet snippets/example.cpp Adding a resource`

文件名后的文本片段的惟一标识符。这是用来划定相关片段文件中引用的代码如以下示例所示,对应于上述\片段命令:

```C++
    QImage image(64, 64, QImage::Format_RGB32);    image.fill(qRgb(255, 160, 128));//! [Adding a resource]    document->addResource(QTextDocument::ImageResource,        QUrl("mydata://image.png"), QVariant(image));//! [Adding a resource]    ...
```

注意,包含块的线条标记不会被包括,所以输出将会是:

```C++
document->addResource(QTextDocument::ImageResource,    QUrl("mydata://image.png"), QVariant(image));
```

还要注意(block_id)标记应该完全在源文件中出现两次。　　　　
另一种方法参见\ dontinclude包括碎片的源文件不需要标记。

- @until ( pattern )

将最后包含`@include或@dontinclude`的例子中的所有行写入输出，除非它找到一个包含指定模式的行，而只写入包含指定模式的一行。

在例子中会使用一个跟踪当前行的内部指针，将写入输出的那行作为起始行，并设定内部指针。(如果未找到符合要求的模式，指针也将指向例子的末端)

- @verbinclude <file-name>

在文档中完整包含名为`<file-name>`文件，此命令等价于在文档中放置`@verbatim,@endverbatim`命令限定`<file-name>`文件。

- @htmlinclude <file-name>

在HTML文档中完整包含名为`<file-name>`文件，此命令等价于在文档中放置`@htmlonly`

- @latexinclude <file-name>

　这个命令包括文件<文件名>文档。命令相当于粘贴文件文档和放置`@ latexonly和@endlatexonly`命令。　　　
　文件或目录,doxygen应该寻找可以指定使用doxygen `EXAMPLE_PATH`标签的配置文件。