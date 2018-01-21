---
layout:     post
title:      "数据库学习（四）"
subtitle:   "2017/07/22 高级查询"
date:       2017-07-22
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170719.jpg?raw=true"
tags:
    - Database
---

### 时间:2017年7月22日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

# SQL高级查询

## 子查询
当前查询需要建立在另一个查询的结果基础之上，这里就要利用到子查询。
子查询是一条SELECT语句，其查询语句中嵌套了另一个查询，支持多层嵌套。
一般出现在两个位置:

- FROM 之后，也称==行内视图==，该子查询实质就是一个临时视图
- WHERE之后，作为过滤条件的值

注意：

- 子查询要用括号括起来
- 子查询在FROM之后当做数据表时，可以其别名，作为前缀来限定数据列时，必须给子查询起别名
- 子查询当做过滤条件时，将子查询放在比较运算符右边可以增强可读性
- 子查询当做过滤条件时，单行子查询使用单行运算符，多行子查询使用多行运算符

凡是可以查询的内容都可以创建新表
例如；

```
CREATE TABLE emp_dongdong
AS
SELECT e.name nm,e.gender gd,e.salary*12 ys
FROM emp_dongzhixiao e
```

>注意：创建表中有别名，则新表用该别名作为字段名，
当子查询中的字段含有函数表达式，那么该字段必须使用别名，
比如上面的e.salary*12必须起别名

### DML中使用子查询
例如：

```
DELETE FROM emp_dongzhixiao
WHERE deptno = (SELECT deptno
                FROM emp_dongzhixiao
                WHERE name = 'CLARK')
```

### 子查询不同
根据结果集不同分为
单行单列子查询：> < >= <= = <> 这些比较都只能使用单行单列子查询
多行单列子查询：用于过滤条件，由于查询出多个值，在判断=时要用IN,判断>,>=等操作要配合ANY,ALL
多行多列子查询：常当做一张表看待
例如：

```
SELECT ename,job,deptno
FROM  emp
WHERE 
 deptno IN (SELECT deptno
	        FROM emp
	        WHERE job='SALESMAN')
AND job <> 'SALESMAN'
```

```
### 查看职位比CLERK和SALESMAN工资都高的人
SELECT ename,sal
FROM emp
WHERE
 sal > ALL(SELECT sal FROM  emp
         WHERE job IN('CLERK','SALESMAN') 
        )
```

### EXISTS关键字
EXISTS 指定一个子查询，检测行的存在。
语法：EXISTS subquery。
参数 subquery 是一个受限的 SELECT 语句(不允许有 COMPUTE 子句和 INTO 关键字)。
结果类型为 Boolean，如果子查询包含行，则返回 TRUE。



```
# 查看每个部门的最低薪水且最低薪水高于30号部门：
SELECT deptno,MIN(sal)
FROM  emp
GROUP BY 
 deptno
HAVING
 MIN(sal) > (SELECT MIN(sal)
               FROM emp
               WHERE deptno=30)
```

### 子查询在FROM部分

- 当子查询是多列子查询，通常将该子查询结果当做一张表看待并给它进行二次查询
例如:

```
# 查询比自己部门平均工资高的那些员工:
# 1:查看每个部门平均工资
SELECT deptno , AVG(sal)
FROM emp
GROUP BY deptno
# 2: 将结果放到FROM里面
SELECT 
  e.ename,e.sal,e.deptno
FROM
  emp e,(SELECT deptno , AVG(sal) avgsal
         FROM emp
         GROUP BY deptno) b
WHERE
  e.deptno = b.deptno
AND
  e.sal > b.avgsal
```

在from子句中出现了子查询，那么意味着我们要将子查询的结果当做一张表去看待，从中再次查询想要的结果。

### 子查询的SELECT语句中
出现了非字段名的字段(通常是表达式或者函数)，那么一定要给他们加上别名。
例如：

```
SELECT ename,sal,
       (
           SELECT dname 
           FROM dept d 
           WHERE d.deptno = e.deptno
       ) deptno
FROM emp e
```

## 分页查询
分页查询是将查询表中数据分段查询，而不是一次性将所有数据查询出来。
数据库基本都支持分页查询，但是不同数据库语法各部相同

分页的三步:

- 1:排序
- 2:编号
- 3:取范围

MySQL是用limit函数来分页查询
取前5条数据
`select * from table_name limit 0,5 `
或者
`select * from table_name limit 5 `
查询第11到第15条数据
`select * from table_name limit 10,5`
limit关键字的用法：
LIMIT [offset,] rows
offset指定要返回的第一行的偏移量，rows第二个指定返回行的最大数目。初始行的偏移量是0(不是1)。


分页的算法：page页数    pagesize一页的条数
(page-1)*pagesize+1  start   
page*pagesize         end     

## 条件判断函数
### 1、IF(expr,v1,v2)函数

如果表达式expr成立，返回结果v1；否则，返回结果v2。

```
SELECT IF(1 > 0,'正确','错误')    
->正确
```

### 2、IFNULL(v1,v2)函数

如果v1的值不为NULL，则返回v1，否则返回v2。

```
SELECT IFNULL(null,'Hello Word')
->Hello Word
```

### 3、CASE

语法1：

```
CASE 
　　WHEN e1
　　THEN v1
　　WHEN e2
　　THEN e2
　　...
　　ELSE vn
END
```

　　CASE表示函数开始，END表示函数结束。如果e1成立，则返回v1,如果e2成立，则返回v2，当全部不成立则返回vn，而当有一个成立之后，后面的就不执行了。

```
SELECT CASE 
　　WHEN 1 > 0
　　THEN '1 > 0'
　　WHEN 2 > 0
　　THEN '2 > 0'
　　ELSE '3 > 0'
　　END
->1 > 0
```

语法2：

CASE expr 
　　WHEN e1 THEN v1
　　WHEN e1 THEN v1
　　...
　　ELSE vn
END
　　如果表达式expr的值等于e1，返回v1；如果等于e2,则返回e2。否则返回vn。

SELECT CASE 1 
　　WHEN 1 THEN '我是1'
　　WHEN 2 THEN '我是2'
ELSE '你是谁'
 
## 排序函数
允许对结果集按照指定的字段分组，在组内在按照指定的字段排序，最终生成组内编号

### row_number():生成组内连续且唯一的编号
SELECT 
 ename,sal,deptno,
 ROW_NUMBER() 
     OVER(PARTITION BY deptno 
            ORDER BY sal DESC) rank
FROM 
 emp

### rank():生成组内不连续且不唯一的编号
       排序的列若值相同，会得到相同的编号
SELECT 
 ename,sal,deptno,
 RANK() 
     OVER(PARTITION BY deptno 
            ORDER BY sal DESC) rank
FROM 
 emp

### dense_rank():生成组内连续但不唯一的编号
       排序的列若值相同，会得到相同的编号
SELECT 
 ename,sal,deptno,
 DENSE_RANK() 
     OVER(PARTITION BY deptno 
            ORDER BY sal DESC) rank
FROM 
 emp

SELECT 
  year_id,month_id,day_id,SUM(sales_value)
FROM sales_tab
GROUP BY
   GROUPING SETS(
      (year_id,month_id,day_id),
      (year_id,month_id)
   )
ORDER BY year_id,month_id,day_id

