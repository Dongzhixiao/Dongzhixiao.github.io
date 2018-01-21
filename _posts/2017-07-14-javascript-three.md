---
layout:     post
title:      "JavaScript学习（三）"
subtitle:   "2017/07/14 JavaScript"
date:       2017-07-14
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170714.jpg?raw=true"
tags:
    - JavaScript
---

### 时间:2017年7月14日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

#一、DOM编程
##1.什么是DOM?

DOM[Document Object Model],文档对象模型。

DOM提供处理XML/HTML文档的API。

DOM的主要操作:节点的获取、节点的动态的创建、创建的删除及节点的替换。

节点(Node),在DOM树中所存在的任何一个元素(如HTML元素，文本、属性等)。

节点的类型

Node.ELEMENT_NODE,1(元素类型)
Node.ATTRIBUTE_NODE,2(属性类型)
Node.TEXT_NODE,3(文本类型)
Node.COMMENT_NODE,8(注释类型)
Node.DOCUMENT_NODE,9(文档类型)


##2.document对象
属性

方法
getElementById()
描述:根据ID获取对象
语法:Element document.getElementById(string id)

createElement()

描述:创建元素节点

语法:Element document.createElement(string tagName)

createTextNode
描述:创建文本节点
语法:textNode document.createTextNode(string value)

createComment
描述:创建注释节点
语法:commentNode document.createComment(string value)

createAttribute
描述:创建属性节点
语法:attrNode document.createAttribute(string name)

3.Node接口

属性
firstChild
lastChild
nextSibling
previousSibling
parentNode
childNodes
nodeName
nodeType
nodeValue

方法
appendChild()
描述:追加子节点
语法:object.appendChild(node)

insertBefore()
描述:插入子节点
语法:object.insertBefore(newNode[,refNode])

removeChild()
描述:删除节点
语法:object.removeChild(node)

replaceChild()
描述:节点的替换
语法:object.replaceChild(newNode,oldNode)
