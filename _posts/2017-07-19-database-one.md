---
layout:     post
title:      "数据库学习（一）"
subtitle:   "2017/07/19 基本原理"
date:       2017-07-19
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170719.jpg?raw=true"
tags:
    - Database
---

### 时间:2017年7月19日 天气:阴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

# SQL(StructuredQueryLanguage)    
相当于画一个表格----先画列，再画行
处于可读性的考虑，一般可以关键字全部大写，非关键字全部小写
(注：后面的例子使用的软件是MySQL/Navicat for MySOL)

## DDL(DataDefinitionLanguage,数据定义语言)
DDL是对数据库对象进行操作的语言，数据库对象包括
表、视图、索引、序列。

### 创建表的语法
create table ：列 表结构
表名(
列名 数据类型长度 约束，
列名 数据类型长度 约束，
。。。。
)
例如：

```
CREATE TABLE employee_dongzhixiao(
	id INT(4),
	name VARCHAR(20),
	gender CHAR(1),
	birth DATE,
  	salary INT(6),
  	job VARCHAR(30),
  	deptno INT(2)
);
```

### 查询表语法
DESC table_name:查看表结构
看到表的列的名字，以及对应的类型，长度等
例如：`DESC employee_dongzhixiao;`


### 删除一张表的语法
DROP TABLE table_name
例如：`DROP TABLE employee_dongzhixiao`

### DEFAULT关键字
用于为给定的列(字段)设置默认值
例如：

```
CREATE TABLE employee_dongzhixiao(
    id INT(4),
    name VARCHAR(20) NOT NULL,
    gender CHAR(1) DEFAULT 'M',
    birth DATE,
    salary INT(6),
    job VARCHAR(30),
    deptno INT(2)
);
```

数据库中字段无论是什么类型，默认值都是NULL
若使用DEFAULT指定了默认值，则使用指定的。
数据库中的字符串字面量是使用单引号的，虽然SQL语句
本身不区分大小写，但是字符串值是区分大小写的！

### NOT NULL约束
在创建表的时候可以为列添加非空约束，被约束的
列在插入数据时必须给值。此列不允许为空。

### 修改表名
RENAME TABLE old_name TO new_name

需要注意：新的表名不能是数据库中现有的表
例如：
`RENAME TABLE employee_dongzhixiao TO emp_dongzhixiao`


### 修改表:
为表添加新的字段(列),总是在表的最后一列追加
ALTER TABLE emp_dongzhixiao ADD
(
	#可以有多个列定义
	column_name1 datatype [DEFAULT expr],
	...
)

## ADD (hiredate DATE DEFAULT sysdate);

sysdate是一个日期的值，表示当前系统时间。


### 从表中删除一列
ALTER TABLE emp_dongzhixiao
DROP (hiredate);

### 修改表中现有的列
ALTER TABLE emp_dongzhixiao
MODIFY job VARCHAR(40) DEFAULT 'CLERK'
注意：MySQL不支持一次修改多个列，其他数据库如Oracle支持一个MODIFY命令修改多个列定义
那样需要加在MODIFY后加上括号，如果想要MsSQL修改多个列，则可在MODIFY后使用拖个MODIFY命令   

修改表字段时的注意事项:
1:尽量不修改字段类型。
2:字段长度尽量不要减少。
3:修改后的字段，只对新插入的数据产生影响，
   修改字段前的所有数据不影响。

## DML操作
### 向表中插入数据
INSERT INTO table_name
VALUES(1,'冬之晓',28,'男',0)

INSERT INTO 
   emp_dongzhixiao(id,name,salary)
VALUES(1,'boss',1500)

INSERT语句是向表中插入数据
INSERT语句指定的列对应的值会被插入到表中
没有列举的列会插入NULL,但是，若该列有设置
默认值(DEFAULT关键字设置的)，那么就插入
设置的默认值。
若某列为NOT NULL,执行INSERT语句时又没有
指定该列，那么插入会抛出违反为空约束的异常

执行INSERT语句时，若没有指定插入任何列，那
么就是全列插入，注意，给的值顺序必须与表中
列的顺序完全一致，并且不能忽略任何一个列的
值

例如：
```
INSERT INTO emp_dongzhixiao(id,name,salary)
VALUES(2,'tom',2500)

INSERT INTO emp_dongzhixiao(id,name,salary)
VALUES(3,'JERRY',3500)
```

### 查询表数据
SELECT   *    FROM    emp_dongzhixiao

### 事务控制:

- COMMIT
用于提交事务。

- ROLLBACK
用于回滚事务。那么本次事务中所有的增删改操作
全部失效。

### 日期格式化函数
str_to_date('2012-05-01 23:59:59','%Y-%m-%d %T')
 上函数转换为DATE数据类型----用于表示 年月日，如果实际应用值需要保存 年月日 就可以使用 DATE。 
 
- %Y：代表4位的年份
- %y：代表2为的年份
 
- %m：代表月, 格式为(01……12)  
- %c：代表月, 格式为(1……12)
 
- %d：代表月份中的天数,格式为(00……31)  
- %e：代表月份中的天数, 格式为(0……31) 
 
- %H：代表小时,格式为(00……23)  
- %k：代表 小时,格式为(0……23)  
- %h： 代表小时,格式为(01……12)  
- %I： 代表小时,格式为(01……12)  
- %l ：代表小时,格式为(1……12)
  
- %i： 代表分钟, 格式为(00……59) 

- %r：代表 时间,格式为12 小时(hh:mm:ss [AP]M)  
- %T：代表 时间,格式为24 小时(hh:mm:ss) 

- %S：代表 秒,格式为(00……59)  
- %s：代表 秒,格式为(00……59) 

例如：
```
INSERT INTO emp_dongzhixiao
  (id,name,birth)
VALUES
  (1,'jack',str_to_date('1989-11-08','%Y-%m-%d'))
```

### 修改表中的数据

UPDATE table_name
SET  column1 = value1[, column2 = value2,....]
[WHEN condition];

例如：
```
UPDATE emp_dongzhixiao
SET job='MANAGER' 
WHERE salary=8500;
```
>注意：where句的作用----可以控制只选择指定行。
where包含的条件表达式可以使用>、>=、<、<=、=、<>等基本运算符
SQL中的比较可以比较数值、字符串、日期的大小。
其中判断两个是是否相等是==单等号==，判断不相等是<>，赋值运算符是冒号等号(:=)

>注意:通常情况下，更改表时，要添加WHERE
来指定过滤条件，若不指定WHERE则是全表修改
通常不会这样做。

### 从表中删除数据

DELETE FROM table_name
[WHEN condition];

例如：
```
DELETE FROM emp_dongzhixiao
WHERE name='tom' 
```

删除数据时更要注意，添加WHERE.否则是全表
删除。

>注意区别TRUNCATE的删除效果
属于DDL的删除语句，直接删，不能回退了！
例如：TRUNCATE TABLE emp_dongzhixiao;





