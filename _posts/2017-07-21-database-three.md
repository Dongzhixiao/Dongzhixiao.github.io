---
layout:     post
title:      "数据库学习（三）"
subtitle:   "2017/07/21 基础查询和关联查询"
date:       2017-07-21
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170719.jpg?raw=true"
tags:
    - Database
---

### 时间:2017年7月21日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------


## DQL查询语句
当SELECT中的字段是一个表达式，那么查询出来
的结果集中，当前列的列明就是该表达式，这样的
可读性差，为此我们可以使用别名来改善。
例如；`SELECT id "another name" FROM emp_dongzhixiao`
上别名没有空格可以不加双引号

### WHERE句子
有时候操作数据库时，只操作一些有条件限制的数据，这时可以在SQL语句中添加WHERE子句来规定数据操作的条件。

where包含的条件表达式可以使用>、>=、<、<=、=、<>等基本运算符
SQL中的比较可以比较数值、字符串、日期的大小。
其中判断两个是是否相等是==单等号==，判断不相等是<>，赋值运算符是冒号等号(:=)
查询中的条件使用AND/OR,表示
都为真才真/都为假才假
例如：

```
SELECT name, salary, job 
FROM emp_dongzhixiao
WHERE 
 salary > 1250 
AND 
  (job = 'CLERK'
OR
  job = 'SALESMAN')
```

AND优先级是高于OR的。因此要加括号来提升OR的优先级

### 模糊查询
LIKE
_:  单一的一个字符
%：表示任意个或多个字符。可匹配任意类型和长度的字符。

###判断是否存在
IN和NOT IN
判断是否咋列表中或不在列表中
例如：

```
SELECT name, salary, job 
FROM emp_dongzhixiao 
WHERE 
 job IN ('MANAGER','SALESMAN','CLERK')
```

### 如何查询两个日期之间的记录
日期所在字段的类型为datetime（0000-00-00 00:00:00）
解决方案：
直接使用><=就可以查询。
`where createDate<'2003-5-31' and createDate>'2003-2-30';`
可以使用BETWEEN/AND
例如：`WHERE LogTime BETWEEN '2010-08-01' AND '2010-08-19'; `

### 使用ANY和ALL条件
ANY和ALL不能单独使用，需要配合单行比较操作符<、>、<=、>=同时使用
其意义是：

- `>ANY(list) 大于最小的`
- `<ANY(list) 小于最大的`
- `>ALL(list) 大于最大的`
- `<ALL(list) 小于最小的`

与in的相同之处:给定一组数据进行比较
区别:in是与给定的数据进行等值或不等值比较any和all是与给定数据进行范围比较 

例如：

```
SELECT name, job, salary
FROM emp_dongzhixiao
WHERE sal > ALL (2500,4000,4500);
```

### 查询中的函数表达式
查询中可以使用函数作为条件
例如：

```
SELECT 
  name, job, salary
FROM 
  emp_dongzhixiao
WHERE 
  sal*12 > 100000
AND
  name = UPPER('BoSs');
```

### 查询去除重复操作
DISTINCT----对结果中指定自段重复的记录进行去重

例如：

```
SELECT DISTINCT deptno,job
FROM emp_dongzhixiao
```

>注意：对单列去重，就一列的值没有重复的
对队列去重，可以达到的效果是，这几列的组合
是不重复的。


### 查询排列操作
ORDER BY 字句 ASC/DESC代表：
根据后面指定的字段对结果进行升序列/降序排序
例如：

```
SELECT name, job, salary
FROM emp_dongzhixiao
ORDER BY salary,id DESC
```

>注意：使用多列进行排序时，左面的列排序优先级高于
右面的列
以上面为例:
首先按照sal的升序排列，当sal的值相同时，按照
deptno的降序排列。若salary的值全表没有重复，那么
第二列的排序会被忽略。排序的字段中含有NULL，被认作最大值


### 聚合函数
又名多行函数，分组函数
是对结果集某些字段的值进行统计的
#### MAX,MIN函数
求给定字段的最大值和最小值
例如：

```
SELECT MAX(sal) max_sal,MIN(sal) min_sal
FROM emp_dongzhixiao
```

#### AVG,SUM函数
求平均值和总和
例如：

```
SELECT AVG(comm)
FROM emp_dongzhixiao

SELECT AVG(NVL(comm,0))
FROM emp_dongzhixiao
```

NVL是把NULL的值变为0，这样就可以计算平均值之类的结果了

#### COUNT函数
COUNT：用于统计记录条数
例如：

```
SELECT COUNT(*)  
FROM emp_dongzhixiao
WHERE salary=20
```

>星号符号(*)代表查看着张表的记录总数

### 分组
GROUP BY
例如：

```
SELECT 
  MAX(salary),MIN(salary),AVG(salary),SUM(salary)
FROM
  emp_fanchuanqi
GROUP BY
  deptno
```

>在表中，deptno值相同的记录将被看做是一组。
查看每个部门中每种职位的工资情况

例如：

```
SELECT 
  MAX(sal),MIN(sal),AVG(sal),
  SUM(sal)
FROM
  emp_dongzhixiao
GROUP BY
  deptno,job
```

>若GROUP BY中出现了多列，那么就按照
这几列组合值相同的记录看做一组

只要在SELECT中使用了分组函数，那么，
SELECT中其他非分组函数的列若出现，那么
也必须同时出现在GROUP BY子句中。
反过来没有限制。



例如：查看平均工资超过1800的部门，以及平均工资
可以：

```
SELECT 
  deptno,AVG(sal)
FROM 
  emp
GROUP BY
  deptno
 
SELECT 
  deptno,AVG(sal)
FROM 
  emp_fanchuanqi
GROUP BY
  deptno
HAVING
  AVG(sal)>1800
```

### HAVING和WHERE过滤函数
WHERE：
不能使用聚合函数作为过滤条件，因为过滤时机不对
WHERE是在数据库检索表中数据时，对数据逐条过滤以决定是否查询出
使用聚合函数查询，必须从表中查询完毕后才能得到结果，因此这个过滤时机在WHERE之后
因此要使用HAVING：
HAVING用于在进行分组查询后，二次过滤数据的
HAVING不能独立存在，必须跟在GROUP BY
之后。
HAVING中可以使用组函数的结果进行过滤
与WHERE的区别在于:
WHERE是用于第一次检索数据时过滤的。
HAVING是用于在检索后，进行二次过滤的
例如：

```
SELECT AVG(salary)
FROM emp_dongzhixiao
GROUP BY dept_dongzhixiao
HAVING AVG(salary)>2000
```

### 总结：查询语句执行顺序

1.from字句：执行顺序从后往前，从右到左

- 数据量较少的表尽量放在后面

2.where字句：执行顺序从上而下、从右到左

- 能过滤掉最大数量记录的条件写在wherer字句的最右
SELECT e.ename,d.dname

3.group by----执行顺序从左往右分组

- 最好在group by前使用wherer将不需要的记录在group by之前过滤掉

4.having字句：消耗资源

- 尽量避免使用，having会在检索出所有记录之后才对结果进行过滤，需要排序等操作

5.select字句：少用*号，尽量取字段名称。

- 数据库解析过程中，通过查询数据字典将*号依次转换成所有列明，消耗时间

6.order by字句：执行顺序为从左到右排序，消耗资源

## 关联查询
从多张表中查询对应记录的信息
关联的重点在于这些表中的记录的对应关系，这个关系也叫==连接条件==
关联查询要添加连接条件，否则会产生笛卡尔积
笛卡尔积通常是一个无意义的结果集，它的记录数是所有参加查询的记录数乘积的结果
要避免出现，数据量大时极易出现内存溢出现象
N张表关联查询至少有N-1个连接条件
例如：

```
FROM emp_dongzhixiao e,dept_dongzhixiao d
WHERE 
 e.deptno=d.deptno
AND
 d.dname='SALES'

SELECT d.loc
FROM emp_dongzhixiao e,dept_dongzhixiao d
WHERE
 e.deptno = d.deptno
AND
 e.ename='FORD'

SELECT e.name,d.dname
FROM emp_dongzhixiao e,dept_dongzhixiao d
WHERE
 e.deptno = d.deptno
ADN
 e.ename='FORD'    
<==>
SELECT e.name,d.dname
FROM emp_dongzhixiao e JOIN dept_dongzhixiao d
ON e.deptno = d.deptno
WHERE e.ename='FORD'  
(后面这种层次感更好----加入后紧接着写关联条件，使得where只写过滤条件)

SELECT e.name,d.dname
FROM emp_dongzhixiao e 
NATURAL JOIN dept_dongzhixiao
```

>关联查询也可以使用别名
当两张表有同门字段是，必须明确指明字段属于哪张表

>自然连接：会自动寻找两张表列名相同的做等值连接。
注意，两张表中应当只有一列名字相同才可以使用自然连接。

>内连接：内连接也是用来完成关联查询

外连接
外连接除了会列出满足条件的查询，还会列出不满足连接条件的记录
外连接的应用场景：当我们查看部门表时，因为在进行与emp连接查询时，40号部门不存在任何员工，导致查询结果该记录被忽略。
这时若我们需要主要查看部门有哪些时，就使用外连接。
外链接分为：

- 左外链接：(LEFT OUTER JOIN)以JOIN左侧表作为驱动表(所有数据都会被查询出来)，那么，
当该表中的某条记录不满足连接条件时来自右侧表中的字段全部填NULL
- 右外链接：(RIGHT OUTER JOIN)
- 全外连接：(FULL OUTER JOIN)
例如：

```
SELECT 
  e.ename,d.dname
FROM 
  emp_dongzhixiao e 
LEFT OUTER JOIN 
  dept_dongzhixiao d
ON(d.deptno=e.deptno)


UPDATE emp_dongzhixiao
SET deptno=50
WHERE ename='FORD'
```

自连接
即：当前表的一条记录可以对应当前表自己的多条记录
自连接为了解决同类型数据但是有存在上下级关系的树状结构的数据
例如：

```
SELECT e.name, m.name
FROM emp_dongzhixiao e, emp_dongzhixiao m
WHERE e.mgr=m,empno
```
