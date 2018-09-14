---
layout:     post
title:      "JavaScript学习（一）"
subtitle:   "2017/07/12 JavaScript"
date:       2017-07-12
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170716.jpg?raw=true"
tags:
    - JavaScript
---

### 时间:2017年7月12日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

# 一、JavaScript基础

## 1.什么是JavaScript?

JavaScript是一种客户端运行的解释性脚本语言。

JavaScript是由网景(Netscape)推出的产品。

Microsoft推出的JScript。

ECMAScript(欧洲计算机制造商协会),

## 2.JavaScript能做什么?

完成客户端的交互工作(如表单的验证、焦点广告、菜单效果等)。

## 3.JavaScript的使用方式

### 3.1 使用外部的JS文件
  
    JavaScript文件的扩展名.js
  
    <script type="text/javascript" src="JS文档URL"></script>

### 3.2 书写于文档的头部

   <script type="text/javascript">
      ...
      ...
   </script>
 
## 4.JS代码规范

A.可选的分号(但一般情况下都需要分号结尾)

B.大小写敏感

C.每行代码尽量不要超过80个字符。

## 5.标识符

  指语言环境下的变量名称、类名称、对象名称等。

A.标识符必须以字母或下划线开头,包含字母、数

字及下划线。

B.标识符禁止包含空格、斜线等特殊符号。

C.标识符禁止与系统关键字相同。

## 6.变量
[var] 变量名称;

[var] 变量名称 = 值;

说明:建议在声明变量时使用var关键字。

# 二、数据类型

## 1.字符型(string),必须括在单引号/双引号之间。

  转义符:
  \n,换行
  \r,回车
  \t,水平制表符
  \v,垂直制表符
  \\,反斜线
  \',单引号
  \",双引号

## 2.数值型(Number),可以存储整数或浮点数,
可以带有符号位。

## 3.布尔型(Boolean),只有true和false。

## 4.数据类型的自动转换

字符+数字:数字转换成字符

数字+布尔:布尔转换成数字(true=>1,false=>0)

字符+布尔:布尔转换成字符(true=>"true",false=>"false")

布尔+布尔:布尔转换成数字(true=>1,false=>0)

## 5.数据类型的强制转换

parseInt,转换成整数

parseFloat,转换成浮点型


## 6.JavaScript的调试工具(补)

Firebug(F12) --> Console(控制台)

## 7.运算符

字符运算符:+
算术运算符:+(正数)、-(负数)、*、/、%、+、-、
逻辑运算符:!、&&、||

比较运算符:>、>=、==、!=、<>、===(全等)、!==(不全等)、<=、<

全等:值与数据类型完全匹配。

自增/自减运算符:
  i++,i--(后缀形式:先使用,后加减)
  ++i,--i(前缀形式:先加减,后使用)

三目运算符: 表达式? 值:值;

流程控制:
if
if...else
if...else if...else
switch
for
while
do...while
