---
layout:     post
title:      "JavaScript学习（四）"
subtitle:   "2017/07/15 JavaScript"
date:       2017-07-15
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170716.jpg?raw=true"
tags:
    - JavaScript
---

### 时间:2017年7月15日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

# 一、HTMLDOM

## 1.什么是HTMLDOM?

HTMLDOM提供处理HTML文档的API。

## 2.W3CDOM与HTMLDOM的区别

W3CDOM可以处理HTML/XML文档;

HTMLDOM仅能处理HTML文档。

## 3.获取对象

A.document.getElementById(string id)

B.Element.getElementsByTagName
(string tagName)

    返回由标记名称所组成的集合(NodeList)。
    
C.document.getElementsByName
(string name)

  返回具有相同name属性的对象所组成的集合
 (主要用于复选框)


## 4.访问HTML对象的属性

object.属性名称 = 值

[var 变量名称 = ] object.属性名称

说明:
A.HTML标记的属性即HTMLDOM节点的属性。

B.如果HTML标记的属性为合成词,在HTMLDOM中应采用"驼峰标记法"命名。

C.HTML标记的class属性,在HTMLDOM中应使用className取代。(因为class是ECMAScript预保留的关键字)

D.HTML标记的style属性,在HTMLDOM中将返回
CSSStyleDecleration(或CSS2Properties)对象。

## 5.CSSStyleDecleration对象

访问CSS样式

CSSStyleDeclaration.属性名称 = 值

[var 变量名称 = ] CSSStyleDeclaration.属性名称 = 值

说明:
A.如果CSS样式为单个单词，则在CSSStyleDeclaration对象中直接书写。

B.如果CSS样式带有短横线,则在CSSStyleDeclaration对象中去掉短横线，然后再使用"驼峰标记法"命名。

C.CSS样式中的float属性在CSSStyleDeclaration对象中,如果浏览器为Chrome、Firefox等，则使用cssFloat取代;如果浏览器为IE则使用styleFloat取代。

## 6.访问HTML对象的文本

所有文本都认为纯文本(HTML不能被解析)
object.innerText

HTML可以被解析
object.innerHTML

## 7.添加节点

A.全部HTMLDOM节点的创建都可以通过W3CDOM的方法实现

B.有几个特殊的HTMLDOM节点，它们拥有自己
的创建、删除方法。

### 7.1 图像

通过构造函数方式

[var 变量名称 = ] new Image(width,height)

### 7.2 列表框

A.列表框

add()方法

描述:添加Option对象

语法:object.add(optionElement)

remove()方法
描述:删除Option对象
语法:object.remove(index)

options属性

描述:返回列表框中所有列表项的集合

语法:object.options

value
描述:返回列表框中被选定选项的值

语法:string object.value



B.列表选项

创建列表选项对象(Option对象) -- 构造函数方式

[var 变量名称 = ] new Option(text[,value[,defaultSelected[,selected]]])

text,指列表项显示文本

value,指列表项的提交值,如果省略value,则提交值与显示文本相同。


defaultSelected,指是否为默认选项(boolean)

selected,指是否被选定
(boolean)

## 8.单选框/复选框/列表框

说明:

A.单选框/复选框在HTML中默认被选定，需

要使用checked="checked"属性;在HTMLDOM编程时

需要使有object.checked = boolean语句。

B.所有表单控件(如单行文本框、密码框等)都存

在disabled="disabled"属性(禁用);在HTMLDOM

编程时使用object.disabled = boolean语句。

HTMLDOM整体方法图像可以如下图显示：
![HTMLDOM](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/HTMLDOM.png?raw=true "HTMLDOM整体方法图像")

