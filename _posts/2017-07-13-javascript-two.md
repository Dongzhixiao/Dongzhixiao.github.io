---
layout:     post
title:      "JavaScript学习（二）"
subtitle:   "2017/07/13 JavaScript"
date:       2017-07-13
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170713.jpg?raw=true"
tags:
    - JavaScript
---

### 时间:2017年7月13日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

# 一、JavaScript的内置对象

## 1.String

创建String对象

直接量方式
var object = '值';
var object = "值";

构造函数方式
var object  = new String("value");

属性
length
描述:获取字符串的长度
语法:int object.length


方法
toLowerCase()
描述:转换成小写字母
语法:string object.toLowerCase()

toUpperCase()
描述:转换成大写字母
语法:string object.toUpperCase()

substr() [补P17[2]]
描述:截取字符串
语法:string object.substr(int start[,int length])
说明:
A.字符从0开始编号
B.起始位置为负数,则倒数

substring()
描述:截取字符串
语法:string object.substring(start[,end])
说明:包含起始位置,但不包含结束位置。

indexOf()
描述:返回一个字符串在另一个字符串第一次出现的位置
语法:int object.indexOf(string str[,int start])
说明:如果没有出现则返回-1

lastIndexOf()
描述:返回一个字符串在另一个字符串最后一次出现的位置
语法:int object.lastIndexOf(string str[,int start])
说明:如果没有出现则返回-1

charAt(int pos) 等价于 substr(int pos,1)

replace()
描述:字符替换
语法:string object.replace(object regExp,string replacement)

split
描述:将字符串拆分成数组
语法:array object.split(string separator)

## 2.Math

属性
Math.PI
Math.SQRT2

方法
Math.ceil()
描述:向上取整
语法:int Math.ceil(float val)
Math.floor()
描述:向下取整
语法:int Math.floor(float val)

Math.pow()
描述:幂运算
语法:float Math.pow(float base ,float exp)

Math.sqrt()
描述:平方
语法:float Math.sqrt(float val)

Math.min()
描述:返回最小值
语法:float Math.min(float val,float val,....)
Math.max()
描述:返回最大值
语法:float Math.max(float val,float val,....)

Math.round()
描述:四舍五入
语法:float Math.round(float val)
说明:保留到整数位。

Math.random()
描述:产生随机数
语法:float Math.random()

## 3.Array

创建数组

 直接量方式
 var object = [值,....]
 构建函数方式
 var object  = new Array(值,...)
 属性
 length
 描述:返回数组成员的数量
 语法:int object.length

  访问数组成员 
  数组名称[下标]  
  说明:数组的下标从0开始。  

   for...in语句
   作用:遍历数组/对象
   语法:
   for(变量名称 in 数组/对象){
        ...  
    }

    方法
   
    join()
    描述:将数组成员连接成字符串 
    语法:string object.join([string separator])

    push()

    描述:在数组的未尾添加一个或多个成员
    语法:int object.push(val,...)

    unshift()
    描述:在数组的开头添加一个或多个成员
    语法:int object.unshift(val,...)

    pop()
    描述:删除数组的最后一个成员，并且返回该成员
    语法:val object.pop()
    shift()
    描述:删除数组的第一个成员，并且返回该成员
    语法:val object.shift()

    slice()
    描述:截取数组
    语法:array object.slice(start[,end])

    reverse()  
    描述:数组反转
    语法:array object.reverse()

## 4.Date

创建Date对象

var object = new Date()

方法
getYear()
描述:获取年份
语法:int object.getYear()

getFullYear()
描述:获取年份
语法:int object.getFullYear()

getMonth()
描述:获取月份(取值范围为0~11)
语法:int object.getMonth()
getDate()
描述:获取日期(多少号)
语法:int object.getDate()
getDay()
描述:获取星期的第几天(0为星期日,依次类推)
语法:int object.getDay()

getHours()
描述:获取小时
语法:int object.getHours()
getMinutes()
描述:获取分钟
语法:int object.getMinutes()
getSeconds()
描述:获取秒
语法:int object.getSeconds()

getTime()
描述:获取毫秒
语法:int object.getTime()

# 二、自定义函数

## 1.什么是自定义函数

完成某种功能的代码段。

## 2.创建自定义函数

function 函数名称([参数[,...]]){
    ...
    ...
    [return 返回值]
}

## 3.调用自定义函数

[var 变量名称=] 函数名称([值[,...]])

## 4.变量作用域

### 4.1 JS编译和执行过程

A.编译,只负责变量的声明和函数的定义。
   而且所有变量的初始值为undefined.

B.执行,自上而下,
    
### 4.2 变量作用域

全局变量

局部变量

## 5.arguments对象

arguments对象指由函数的参数所组成的对象。

length属性

## 6.匿名函数

没有名称的函数称为匿名函数。

## 7.全局函数
parseInt()
parseFloat()
isNaN()

encodeURI
描述:对于URL地址中的信息进行编码
语法:string encodeURI(string str)

decodeURI
描述:对于URL地址中的信息进行解码
语法:string decodeURI(string str)

其中空格将转换成%20




