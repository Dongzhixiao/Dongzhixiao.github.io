---
layout:     post
title:      "python学习(十一)"
subtitle:   "2017/08/19 数据加载相关函数"
date:       2017-08-19
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年8月19日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

下面主要使用pandas包里面的数据加载函数进行讲解。
## 读写文本格式数据
pandas的数据加载函数表格：
|函数|说明|
|-|-|
|read_csv|从文件、URL、文件型对象中加载带分隔符的数据。默认分隔符为逗号|
|read_table|从文件、URL、文件型对象中加载带分隔符的数据。默认分隔符为制表符(“\t”)|
|read_fwf|读取定宽列格式数据(也就是说，没有分隔符)|
|read_clipboard|读取剪切板中的数据，可以看做read_table的剪切板版。在将网页转换为表格时很有用|
下面说明read_csv常用参数作用：
|参数名|作用|
|-|-|
|header|用作列名的行号。默认为0(第一行)，如果没有header行就应该设置为None|
|names|用于结果的列名列表，结合header=None|
|index_col|用作行索引的列编号或列名。可以是单个名称/数字或由多个名称/数字组成的列表(层次化索引)|
|sep或delimiter|用于对行中各字段进行拆分的字符序列或正则表达式！|
|path|表示文件系统位置、URL、文件型对象的字符串|
|skiprows|需要忽略的行数(从文件开始出算起)，或需要跳过的行号列表(从0开始)|
|na_values|一组用于替换NA的值|
|comment|用于将注释信息从行尾拆分出去的字符(一个或多个)|
|parse_dates|尝试将数据解析为日期，默认为False。如果为True，则尝试解析所有列。此外，还可以指定需要解析的一组列号或列名。如果列表的元素为列表或元组，就会将多个列组合到一起再进行日期解析工作(例如，日期/时间分别位于两个列中)|
|keep_date_col|如果连接多列解析日期，则保持参与连接的列。默认为False|
|converters|由列号/列名跟函数之间的映射关系组成的字典。例如：{'foo':f}会对foo列的所有值应用函数f|
|dayfirst|当解析有歧义的日期时，将其看做国际格式(例如，8/6/2017→June8,2017)。默认为False|
|date_parser|用于解析日期的函数|
|nrows|需要读取的行数(从文件开始处算起)|
|iterator|返回一个TextParser以便逐块读取文件|
|chunksize|文件块的大小(用于迭代)|
|skip_footer|需要忽略的行数(从文件末尾处算起)|
|verbose|打印各种解析器输出信息，比如“非数值列中缺失值的数量”等|
|encoding|用于unicode的文本编码格式。例如，"utf-8"表示用UTF-8编码的文本|
|squeeze|如果数据经解析后仅含一列，则返回Series|
|thousands|千分位分隔符，如“,”或“.”|

例如：
```
In [26]: %pwd
Out[26]: 'C:\\Users\\Administrator'
```
假设在'C:\\Users\\Administrator'路径下有文件'test.abc'以文本格式存储着：
```
x,y,z,tittle
1,2,3,aa
4,5,6,bb
7,8,9,cc
```
则可以这样读取：
```
In [27]: pd.read_csv('test.abc')  #基本读取
Out[27]: 
   x  y  z tittle
0  1  2  3     aa
1  4  5  6     bb
2  7  8  9     cc

In [28]: pd.read_csv('test.abc',names=['x','y','z','tittle'],index_col='tittle')
Out[28]: 
        x  y  z
tittle         
tittle  x  y  z
aa      1  2  3
bb      4  5  6
cc      7  8  9
```