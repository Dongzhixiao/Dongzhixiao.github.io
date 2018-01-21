---
layout:     post
title:      "数据库学习（二）"
subtitle:   "2017/07/20 基本函数"
date:       2017-07-20
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170719.jpg?raw=true"
tags:
    - Database
---

### 时间:2017年7月20日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

## 快速入门SELECT
SELECT句后面跟的是要查询的字段，可以包括表中的具体字段，函数或表达式
FROM句用来指定数据来源的表
WHERE句用来添加过滤条件，这样讲满足条件的记录查询出来
例如：

```
SELECT *(全列查找)
FROM table_name
```

### 查找某几列
SELECT id,name,sal(指定查询表中的某几列)
FROM emp

SELECT * FROM emp_dongzhixiao;

//只查询三列的值
SELECT empno,ename,job
FROM emp_dongzhixiao

SELECT 'hello'||'world'
FROM DUAL(虚表)

### 字符串小知识补充：
"||"在数据库中是连接字符串，相当于java中的"+"
注意和java中的"||"区分。
例如:
 java中  "hello"+"world" ==> "helloworld"
 DB中   ‘hello’||'world' ==> 'helloworld'
oracle中 CONCAT('hello','world') ==>'helloworld'
'think'||'in'||'java'
也可以使用字符串连接函数CONCAT(str1,str2)函数
CONCAT(CONCAT('think','in'),'java')
例如：

```
SELECT ename||':'||salary
FROM emp_dongzhixiao

SELECT CONCAT(CONCAT(ename,':'),salary)
FROM emp_dongzhixiao

SELECT ename,LENGTH(ename)
FROM emp_dongzhixiao
```

### 一些其他函数：
LENGTH(str)函数：查看字符串长度
UPPER(str)/LOWER(str)/INITCAP(str)函数：将字符串转换为
全大写/全小写/首字母大写(可空格隔开多个单词使得每个单词首字母都大写,Oracle有，MySQL没有这个函数)
例如：

```
SELECT 
  UPPER(name), 
  LOWER(name), 
FROM emp_dongzhixiao;
```

### DUAL：虚表，没有这么一个表，只为了满足SELECT的语法要求。
我们常用虚表来测试表达式的结果。
在数据库中，我们想测试某个表达式的结果只能
使用SELECT语句来实现。

什么时候使用虚表:当SELECT语句中没有任何表中的字段参与时

### 去除空白或相同字符的函数

TRIM([remstr FROM] str)/LTRIM(str)/RTRIM(str): 分别代表
去除字符串两边指定重复字符/仅仅去除左边空格/仅仅去除右边空格
参数中from前面只能是单一字符
若没有from以及前面的字符，则是去除空白
例如：

```
SELECT TRIM('e' from 'eeeeeliteeeeee')
FROM DUAL;

SELECT LTRIM('   liteeeee')
FROM DUAL;

SELECT RTRIM('eeeelit    ' FROM 'e')
FROM DUAL;
```

### 补位函数
SELECT LPAD(salary,20,'$')
FROM emp_dongzhixiao
作用:要求显示20个字符，若sal的值不足长度，则
补充若干个'$'，以达到20个字符
例如：

```
SELECT RPAD('aaaaAAAAAA',5,'$')
FROM DUAL
```

### 截取字符串
数据库下标从一开始
例如：

```
SELECT
	SUBSTR('thinking in mysql' FROM -5 FOR 5)
FROM DUAL
最后的5不写着则默认截取到尾部
```

### 查找指定字符串在某一个字段的内容中的位置
INSTR(字段名, 字符串)
这个函数返回字符串在某一个字段的内容中的位置, 没有找到字符串返回0，否则返回位置（从1开始）
例如；

```
SELECT 
  INSTR('Doctor Who Who Who', 'Who') 
FROM DUAL;
```

###用于数据的四舍五入的函数
round函数用于数据的四舍五入，它有两种形式：
- 1、round(x,d)  ，x指要处理的数，d是指保留几位小数
这里有个值得注意的地方是，d可以是负数，这时是指定小数点左边的d位整数位为0,同时小数位均为0；
- 2、round(x)  ,其实就是round(x,0),也就是默认d为0；
例如:

```
1、查询: select round(1123.26723,2);
     结果：1123.27
2、查询: select round(1123.26723,1);
     结果: 1123.3
3、查询: select round(1123.26723,0);
     结果：1123
4、查询: select round(1123.26723,-1);
     结果: 1120
5、查询: select round(1123.26723,-2);
     结果：1100
5、查询: select round(1123.26723);
     结果：1123
```
  
### 求余数函数
MOD(x,y)
返回x除以y以后的余数　　
例如：`SELECT MOD(5,2) -- 1`
 
### 上下取整数的函数
CEIL(x)/FLOOR(x)
返回大于或等于x的最小整数/返回小于或等于x的最大整数　　
例如：

```
SELECT CEIL(1.5) -- 返回2
SELECT FLOOR(1.5) -- 返回1
```

----------------------------------

### 获得当前日期 
CURDATE(),CURRENT_DATE()	
返回当前日期
例如：

```
SELECT CURDATE()
->2014-12-17
```

### 两个日期可以进行减法操作，差为相差的天数。
SELECT CURDATE()-str_to_date('1989-11-08','%Y-%m-%d')

### 查看月底

```
SELECT LAST_DAY(SYSDATE())
->2017-07-31
```

### 查找最大最小值函数
greatest(parm1,parm2,...)
一条记录中取几个字段的最大值
least(parm1,parm2,...)
一条记录中取几个字段的最小值

### 提取时间分量函数
XTRACT(type FROM d)
从日期d中获取指定的值，type指定返回的值
type可取值为：

- MICROSECOND
- SECOND
- MINUTE
- HOUR
- DAY
- WEEK
- MONTH
- QUARTER
- YEAR
- SECOND_MICROSECOND
- MINUTE_MICROSECOND
- MINUTE_SECOND
- HOUR_MICROSECOND
- HOUR_SECOND
- HOUR_MINUTE
- DAY_MICROSECOND
- DAY_SECOND
- DAY_MINUTE
- DAY_HOUR
- YEAR_MONTH

例如：

```
SELECT EXTRACT(MINUTE FROM '2011-11-11 11:11:11') 
->11
```


### 判断是否为NULL
必须使用：IS NOT NULL/IS NULL
例如：

```
SELECT * 
FROM student
WHERE gender IS NOT NULL
任何值都不能等于null
```

### 空值函数
IFNULL(v1,v2)函数
如果v1的值不为NULL，则返回v1，否则返回v2。
例如：

```
SELECT IFNULL(null,'Hello Word')
->Hello Word
```
