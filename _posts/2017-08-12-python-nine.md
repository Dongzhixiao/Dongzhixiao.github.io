---
layout:     post
title:      "python学习(九)"
subtitle:   "2017/08/11 pandas模块相关函数"
date:       2017-08-11
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年8月11日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

## pandas的基本数据结构
### Series结构
类似于一维数组的对象，它由一组Numpy中各种数据类型的数据和一组标签组成。索引在左，值在右：
```
In [1]: import pandas as pd

In [2]: obj = pd.Series([1,2,3,4])

In [3]: obj
Out[3]: 
0    1
1    2
2    3
3    4
dtype: int64
```
>注意：没有指定索引会自动创建0到N-1(N是数据长度)作为索引

Series的values和index属性可以获取数组表示形式和索引对象：
```
In [4]: obj.values
Out[4]: array([1, 2, 3, 4], dtype=int64)

In [5]: obj.index
Out[5]: RangeIndex(start=0, stop=4, step=1)
```
索引可以才创建时自己指定，然后通过一个或多个索引可取得对应值：
```
In [6]: obj = pd.Series([1,2,3,4],index=['a','b','c','d'])

In [7]: obj
Out[7]: 
a    1
b    2
c    3
d    4
dtype: int64

In [8]: obj.index
Out[8]: Index(['a', 'b', 'c', 'd'], dtype='object')

In [9]: obj['a']
Out[9]: 1

In [10]: obj[['c','d']]
Out[10]: 
c    3
d    4
dtype: int64
```
可以使用一下基本运算和numpy相关函数的数组运算，结果保留索引关系：
```
In [11]: import numpy as np

In [12]: obj*2
Out[12]: 
a    2
b    4
c    6
d    8
dtype: int64

In [13]: np.exp(obj)
Out[13]: 
a     2.718282
b     7.389056
c    20.085537
d    54.598150
dtype: float64

In [14]: obj[obj>1]
Out[14]: 
b    2
c    3
d    4
dtype: int64
```

Series可以看做字典，许多字典相关的函数都可用，也可直接通过字典创建Series：
```
In [16]: 'a' in obj
Out[16]: True

In [17]: 'e' in obj
Out[17]: False

In [18]: d = {'a':1,'b':2,'c':3}

In [19]: d
Out[19]: {'a': 1, 'b': 2, 'c': 3}

In [20]: obj = pd.Series(d)

In [21]: obj
Out[21]: 
a    1
b    2
c    3
dtype: int64
```
创建Series还可将键通过列表传入，会自动根据传入的字典查询有的值来生成Series，若字典中的键没有查到列表中的数据，则对应生成Series的键的值为NaN：
```
In [22]: alp = ['c','b','a','d']

In [23]: obj = pd.Series(d,index=alp)  #d={'a': 1, 'b': 2, 'c': 3},即前一个代码片段中的d

In [24]: obj
Out[24]: 
c    3.0
b    2.0
a    1.0
d    NaN
dtype: float64
```

panda的的isnull和notnull函数可以用来查询Series的值是否为NaN：
```
In [26]: pd.isnull(obj)
Out[26]: 
c    False
b    False
a    False
d     True
dtype: bool

In [27]: pd.notnull(obj)
Out[27]: 
c     True
b     True
a     True
d    False
dtype: bool
```

Series对象本身和其索引都有name属性，Series的索引可通过赋值修改：
```
In [28]: obj.name='test'

In [29]: obj.index.name='num'

In [30]: obj
Out[30]: 
num
c    3.0
b    2.0
a    1.0
d    NaN
Name: test, dtype: float64

In [31]: obj.index=['e','f','g','h']

In [32]: obj
Out[32]: 
e    3.0
f    2.0
g    1.0
h    NaN
Name: test, dtype: float64
```

### DataFrame结构
DataFrame是表格形式的数据结构，它含有一个有序的列，每一列可以是不同的值类型。可以看做Series组成的字典公用同一个索引。即有行索引，又有列索引。构建DataFrame的方法较多，最常用的是直接传入一个由等长列表或Numpy数组组成的字典：
```
In [1]: data = {'name':['xd','xc','ch'],
   ...:         'birth':[1989,1988,1991],
   ...:         'amateur':['game','study','work']}
In [3]: frame = pd.DataFrame(data)

In [4]: frame
Out[4]: 
  amateur  birth name
0    game   1989   xd
1   study   1988   xc
2    work   1991   ch
```
又结果看出DataFrame会自动加上索引，且全部有序排列，当然可以自己指定排列顺序：
```
In [6]: pd.DataFrame(data,columns=['birth','name','amateur'])
Out[6]: 
   birth name amateur
0   1989   xd    game
1   1988   xc   study
2   1991   ch    work
```
可以自己定义索引，如果顺序列表中找不到，则会产生NaN：
```
In [7]: pd.DataFrame(data,columns=['birth','name','amateur','gender'],index=['a','b','c'])
Out[7]: 
   birth name amateur gender
a   1989   xd    game    NaN
b   1988   xc   study    NaN
c   1991   ch    work    NaN
```
通过字典标记，或属性方式，可得到对应列的Series数据：
```
In [8]: frame['name']
Out[8]: 
0    xd
1    xc
2    ch
Name: name, dtype: object

In [9]: frame.birth
Out[9]: 
0    1989
1    1988
2    1991
Name: birth, dtype: int64

In [10]: type(frame.birth)
Out[10]: pandas.core.series.Series
```
通过ix方法可以索引一行的字段：
```
In [13]: frame.ix[1]
Out[13]: 
amateur    study
birth       1988
name          xc
Name: 1, dtype: object
```
可通过赋值的方式加上新的列，若通过Series结构赋值，则可精确匹配DataFrame的索引：
```
In [14]: frame
Out[14]: 
  amateur  birth name
0    game   1989   xd
1   study   1988   xc
2    work   1991   ch

In [15]: frame['age'] = 24

In [16]: frame
Out[16]: 
  amateur  birth name  age
0    game   1989   xd   24
1   study   1988   xc   24
2    work   1991   ch   24

In [17]: import numpy as np
In [22]: frame['age'] = np.arange(25.,28.)

In [23]: frame
Out[23]: 
  amateur  birth name   age
0    game   1989   xd  25.0
1   study   1988   xc  26.0
2    work   1991   ch  27.0

In [24]: gen = pd.Series(['M','F'],index=[2,1])

In [25]: frame['gender']=gen

In [26]: frame
Out[26]: 
  amateur  birth name   age gender
0    game   1989   xd  25.0    NaN
1   study   1988   xc  26.0      F
2    work   1991   ch  27.0      M
```
del关键字可以删除列：
```
In [27]: del frame['age']

In [28]: frame
Out[28]: 
  amateur  birth name gender
0    game   1989   xd    NaN
1   study   1988   xc      F
2    work   1991   ch      M
```
>注意：通过索引得到的列是对应数据的视图，因此对返回的Series数据的修改会反应到DataFrame数据上，通过Series的copy方法可以显式的复制对应数据。

另一种构建DataFrame的方式是嵌套字典，外层字典键作为列索引，内存字典键作为行索引，结果也可转置：
```
In [30]: dic = {'MM':{'outfile':'beautiful',2017:6.6},'GG':{'outfile':'handsome',2017:8.8}}
In [32]: frame = pd.DataFrame(dic)

In [33]: frame
Out[33]: 
               GG         MM
outfile  handsome  beautiful
2017          8.8        6.6

In [34]: frame.T
Out[34]: 
      outfile 2017
GG   handsome  8.8
MM  beautiful  6.6
```
下表列出DataFrame可接受的数据：
|类型|说明|
|-|-|
|二维ndarry|数据矩阵，还可以传入行标和列标|
|由数组、列表、元组组成的字典|每个序列会变成DataFrame的一列。所有序列长度必须相同|
|Numpy的结构化/记录数组|类似于“由数组组成的字典”|
|由Series组成的字典|每个Series会成为一列。如果没有显示指定索引，则各Series的索引会被合并成结果的行索引|
|由字典组成的字典|各内层字典会成为一列。键会被合并成结果的行索引，跟“由Series组成的字典”情况一样|
|字典或Series的列表|各项将成为DataFrame的一行。字典键或Series索引的并集将会成为DataFrame的列标|
|由列表或元组组成的列表|类似于“二维ndarry”|
|另一个DataFrame|将会使用该索引，除非自己显示指定|
|Numpy的MaskedArray|类似于“二维ndarry”，只是掩码值在结果DataFrame会变为NaN缺失值|

以上生成时都可自己设置DataFrame的index和columns的name属性，这时会显示出来：
```
In [39]: frame.index.name = 'character'

In [40]: frame.columns.name = 'people'

In [41]: frame
Out[41]: 
people           GG         MM
character                     
outfile    handsome  beautiful
2017            8.8        6.6
```
跟Series数据类似，通过values属性可以返回二维ndarry形式的数据：
```
In [42]: frame.values
Out[42]: 
array([['handsome', 'beautiful'],
       [8.8, 6.6]], dtype=object)
```
### Index索引对象
pandas的索引对象负责管理轴标签和其他元数据，构建Series或DataFrame时，所用的任何数组或其他序列的标签都会转换成一个Index：
```
In [44]: obj = pd.Series(range(3),index=['a','b','c'])

In [45]: index = obj.index

In [46]: index
Out[46]: Index(['a', 'b', 'c'], dtype='object')

In [47]: index[1:]
Out[47]: Index(['b', 'c'], dtype='object')
```
>注意：index是不可修改的。这样才能使得Index对象在多个数据结构之间安全共享。

```
In [50]: index = pd.Index(np.arange(3))
In [53]: obj = pd.Series([1.1,2.2,3.3],index = index)

In [54]: index
Out[54]: Int64Index([0, 1, 2], dtype='int64')

In [55]: index is obj.index
Out[55]: True
```
下面列出pandas内置的Index类：
|类|说明|
|-|-|
|Index|最泛化的Index对象，将轴标签表示为一个由Python对象组成的Numpy数组|
|Int64Index|针对整数的特殊Index|
|MultiIndex|“层次化”索引对象，表示单个轴上的多层索引。可以看做由元组组成的数组|
|DatetimeIndex|存储纳秒级时间戳(用Numpy的datetime64类型表示)|
|PeriodIndex|针对Period数据(时间间隔)的特殊Index|
索引还有一些方法和属性，用于设置逻辑并回答有关该索引所包含的数据的常见问题，下表列出这些函数：
|方法|说明|
|-|-|
|append|连接另一个Index对象，产生一个新的Index|
|difference|计算差集，得到一个Index|
|intersection|计算交集|
|union|计算并集|
|isin|计算一个指示各值是否都包含在参数集中的布尔型数组|
|delete|删除索引i处的元素，并得到新的Index|
|drop|删除传入值，并得到新的Index|
|insert|将索引插入到i处，得到新的Index|
|is_monotonic|当各元素均大于等于前一个元素时，返回True|
|is_unique|当Index没有重复值是，返回True|
|unique|计算Index中唯一值的数组|
例如：
```
In [54]: index
Out[54]: Int64Index([0, 1, 2], dtype='int64')

In [56]: index2 = pd.Index(range(6))
In [57]: index2
Out[57]: RangeIndex(start=0, stop=6, step=1)

In [58]: index2.difference(index)
Out[58]: Int64Index([3, 4, 5], dtype='int64')

In [59]: index2.drop(4)
Out[59]: Int64Index([0, 1, 2, 3, 5], dtype='int64')

In [61]: index2.insert(2,2)
Out[61]: Int64Index([0, 1, 2, 2, 3, 4, 5], dtype='int64')

In [63]: index2.is_monotonic
Out[63]: True

In [64]: index2.is_unique
Out[64]: True

In [66]: index2.unique()
Out[66]: Int64Index([0, 1, 2, 3, 4, 5], dtype='int64')
```

## 基本功能
下面介绍操作Series和DataFrame数据的基本手段
### 重新索引
pandas对象的reindex方法用来创建一个适应新索引的新对象。比如调用Series的reindex方法可以根据新索引重新排序。如果某个索引不存在则引入NaN，当然可以通过参数fill_value改变不存在该索引时的填充值：
```
In [67]: obj
Out[67]: 
0    1.1
1    2.2
2    3.3
dtype: float64

In [68]: obj.reindex([1,2,0])
Out[68]: 
1    2.2
2    3.3
0    1.1
dtype: float64

In [69]: obj  #reindex函数不改变本身，只是生成一个新的
Out[69]: 
0    1.1
1    2.2
2    3.3
dtype: float64

In [70]: obj.reindex([2,1,0,5],fill_value=10)
Out[70]: 
2     3.3
1     2.2
0     1.1
5    10.0
dtype: float64
```
重新索引时还可通过method选项进行向前或向后差值，可设置结果表格：
|参数|说明|
|-|-|
|ffill或pad|前向填充(或搬运)值|
|bfill或backfill|后向填充(或搬运)值|
例如：
```
In [72]: obj = pd.Series(['a','b','c'],index=[2,5,8])

In [73]: obj.reindex(range(8),method='pad')
Out[73]: 
0    NaN
1    NaN
2      a
3      a
4      a
5      b
6      b
7      b
dtype: object

In [74]: obj
Out[74]: 
2    a
5    b
8    c
dtype: object
```
对应DataFrame的reindex函数可修改行、列索引，或同时都修改。仅仅传入序列会重新索引行：
```
In [76]: frame = pd.DataFrame(np.arange(9).reshape((3,3)),index=['a','b','c'],columns=['o','p','q'])

In [77]: frame
Out[77]: 
   o  p  q
a  0  1  2
b  3  4  5
c  6  7  8

In [78]: frame.reindex(['b','c','d','e'])
Out[78]: 
     o    p    q
b  3.0  4.0  5.0
c  6.0  7.0  8.0
d  NaN  NaN  NaN
e  NaN  NaN  NaN
```
显示传入index代表重新索引行，columns代表重新索引列，当然可以混合使用这两个参数：
```
In [5]: frame.reindex(index=['c','b','a'],columns=['q','p','o','j'])
Out[5]: 
     q    p    o   j
c  8.0  7.0  6.0 NaN
b  5.0  4.0  3.0 NaN
a  2.0  1.0  0.0 NaN
```
下面列出reindex函数各个参数的说明表格：
|参数|说明|
|-|-|
|index|用作索引的新序列，可以是Index实例，也可是其他序列的Python数据结构。|
|method|差值的方式，具体见上一个表格|
|fill_value|在重新索引时，需要引入缺失值时使用的代替值|
|limit|前向或后向填充时最大填充量|
|level|在MultiIndex的指定级别上匹配简单索引，否则选取其子集|
|copy|默认为True，无论如何都复制；如果为Flase，则新旧相等则不复制|
### 丢弃轴上值
使用drop函数可返回一个指定轴上删除后的新数据对象，默认是0轴，如果要删除其他轴上的数据，需要显示指定axis数：
```
In [10]: frame
Out[10]: 
   o  p  q
a  0  1  2
b  3  4  5
c  6  7  8

In [11]: frame.drop(['b','c'])
Out[11]: 
   o  p  q
a  0  1  2

In [12]: frame.drop('a')
Out[12]: 
   o  p  q
b  3  4  5
c  6  7  8

In [13]: frame.drop('o',axis=1)
Out[13]: 
   p  q
a  1  2
b  4  5
c  7  8
```
### 索引数据
DataFrame索引选项表格：
|类型|说明|
|-|-|
|obj[val]|选取DataFrame的单个列或一组列。在一些特殊情况下会比较便利：布尔型数组(过滤行)、切片(切片行)、布尔型DataFrame(根据条件设置值)|
|obj.ix/loc[val]|选取DataFrame的单个行或一组行(loc传入的是标签)|
|obj.ix/loc[:,val]|选取单个列或列子集(loc传入的是标签)|
|obj.ix/loc[val1,val2]|同时选取行和列(loc传入的是标签)|
|reindex方法|将一个或多个轴匹配到新索引|
|xs方法|根据标签选取单行或单列，并返回一个Series数据|
|icol、irow方法|根据整数位置选取单列或单行，并返回一个Series数据|
|get_value、set_value方法|根据行标签或列标签选取单个值|
例如：
```
In [31]: frame
Out[31]: 
   o  p  q
a  0  1  2
b  3  4  5
c  6  7  8

In [32]: frame['o']
Out[32]: 
a    0
b    3
c    6
Name: o, dtype: int32

In [33]: frame[frame['o']>0]
Out[33]: 
   o  p  q
b  3  4  5
c  6  7  8

In [34]: frame[:2]
Out[34]: 
   o  p  q
a  0  1  2
b  3  4  5

In [47]: frame.ix[0]
Out[47]: 
o    0
p    1
q    2
Name: a, dtype: int32

In [48]: frame.ix[:,0]
Out[48]: 
a    0
b    3
c    6
Name: o, dtype: int32
```
### 数据结构的简单计算
DataFrame数据进行相加时，不重叠的地方产生NaN值，可以通过其本身的add方法传入相加的值，和fill_value参数：
```
In [66]: frame1 = pd.DataFrame(np.arange(6.).reshape((2,3)),columns=['a','b','c'])
In [69]: frame2 = pd.DataFrame(np.arange(12.).reshape((3,4)),columns=list('abcd'))

In [70]: frame1
Out[70]: 
     a    b    c
0  0.0  1.0  2.0
1  3.0  4.0  5.0

In [71]: frame2
Out[71]: 
     a    b     c     d
0  0.0  1.0   2.0   3.0
1  4.0  5.0   6.0   7.0
2  8.0  9.0  10.0  11.0

In [72]: frame1+frame2
Out[72]: 
     a    b     c   d
0  0.0  2.0   4.0 NaN
1  7.0  9.0  11.0 NaN
2  NaN  NaN   NaN NaN

In [73]: frame1.add(frame2,fill_value=0.)
Out[73]: 
     a    b     c     d
0  0.0  2.0   4.0   3.0
1  7.0  9.0  11.0   7.0
2  8.0  9.0  10.0  11.0

In [75]: frame2-frame1.ix[0]
Out[75]: 
     a    b    c   d
0  0.0  0.0  0.0 NaN
1  4.0  4.0  4.0 NaN
2  8.0  8.0  8.0 NaN
```
### 函数的应用
Numpy的ufuncs(元素级数组方法)也可用来操作pandas对象：
```
In [79]: np.sqrt(frame)
Out[79]: 
          o         p         q
a  0.000000  1.000000  1.414214
b  1.732051  2.000000  2.236068
c  2.449490  2.645751  2.828427
```
还可以直接将函数应用到DataFrame的apply方法上：
```
In [81]: frame.apply(lambda x:x.max()-x.min(),axis=1)
Out[81]: 
a    2
b    2
c    2
dtype: int64
```
也可使用应用到每个元素的方法applymap：
```
In [83]: frame.applymap(lambda x:'%.2f'%x)
Out[83]: 
      o     p     q
a  0.00  1.00  2.00
b  3.00  4.00  5.00
c  6.00  7.00  8.00
```
### 排序
对**索引**排序也是对数据的常用操作，使用的函数是sort_index，该函数可以指定axis参数表示行**索引**排序或列**索引**排序，指定ascending指定升序或降序排序。对值排序可使用sort_values函数，其中指定by代表对一列或多列的**值**进行排序：
```
In [85]: frame.sort_index(axis=1,ascending=False)
Out[85]: 
   q  p  o
a  2  1  0
b  5  4  3
c  8  7  6

In [88]: frame.sort_values(by=['p','o'],ascending=False)
Out[88]: 
   o  p  q
c  6  7  8
b  3  4  5
a  0  1  2
```
### 排名
排名使用rank方法，其参数method对应的选项意思为：
|method|说明|
|-|-|
|'average'|默认：在相等分组中，为各个值分配平均排名|
|'min'|使用整个分组的最小排名|
|'max'|使用整个分组的最大排名|
|'first'|按照原始数据中出现顺序分配排名|
例如：
```
obj = pd.Series([-3,2,7,3,3])

obj.rank()
Out[13]: 
0    1.0
1    2.0
2    5.0
3    3.5
4    3.5
dtype: float64

obj = pd.Series([-3,2,7,3,3,3])

obj.rank()
Out[15]: 
0    1.0
1    2.0
2    6.0
3    4.0
4    4.0
5    4.0
dtype: float64
```
显然，如果出现被排名的出现相同的结果，那么平均方法就按照占有的排名数字的平均数给出排名的序列值。

## 计算描述统计的方法
下面列出描述和汇总统计的常用方法：
|方法|说明|
|-|-|
|count|非NA值的数量|
|describe|针对Series或各DataFrame列计算汇总统计|
|count|非NA值的数量|
|describe|针对Series和DataFrame列计算汇总统计|
|min、max|计算最小值和最大值|
|argmin、argmax|计算能够获取到最小值、最大值的索引位置(整数)|
|idxmin、idxmax|计算能够获取到最小值、最大值的索引值|
|quantile|计算样本的分位数(0到1)|
|sum|值的总和|
|mean|值的平均数|
|median|值的算术中位数(50%分位数)|
|mad|根据平均值计算平均绝对离差|
|var|样本值的方差|
|std|样本值的标准差|
|skew|样本值的偏度(三阶矩)|
|kurt|样本值的峰度(四阶矩)|
|cumsum|样本值的累计和|
|cummin、cummax|样本值的累计最大值和累计最小值|
|cumprod|样本值的累计积|
|diff|计算一阶差分(对时间序列很有用)|
|pct_change|计算百分数变化|
方法的常用选项表格：
|选项|说明|
|axis|DataFrame的行用0，列用1|
|skipna|排除缺失值，默认值为True|
|level|如果轴是层次化索引的(即MultiIndex)，则根据level分组约简|
例如：
```
In [23]: frame = pd.DataFrame([[3.4,np.nan],[4.5,5.6],[np.nan,np.nan],[1.1,4.4]],index=['a','b','c','d'],columns=['one','two'])

In [24]: frame
Out[24]: 
   one  two
a  3.4  NaN
b  4.5  5.6
c  NaN  NaN
d  1.1  4.4

In [25]: frame.sum()
Out[25]: 
one     9.0
two    10.0
dtype: float64

In [26]: frame.sum(axis=1)
Out[26]: 
a     3.4
b    10.1
c     0.0
d     5.5
dtype: float64

In [27]: frame.mean()
Out[27]: 
one    3.0
two    5.0
dtype: float64

In [29]: frame.cumsum()
Out[29]: 
   one   two
a  3.4   NaN
b  7.9   5.6
c  NaN   NaN
d  9.0  10.0
```
### 一些其他常用方法
|方法名|方法作用|
|-|-|
|corr|计算相关系数|
|cov|计算协方差|
|unique|计算Series中唯一值的数组，按发现的顺序返回|
|value_count|返回一个Sereies，其索引为唯一值，其值为各个值出现的频率，按计数值降序排列|
|isin|计算一个表示“Series各值是否包含于传入的值序列中”的布尔型数组|
回忆：
>协方差公式：$$Cov(X,Y)=E[(X-m_1)(Y-m_2)]$$理解：X的方差是$$(X-m_1)$$与$$(X-m_1)$$的乘积的期望，如今把一个$$(X-m_1)$$换为$$(Y-m_2)$$，其形式接近方差，又有X,Y的参与，由此得出协方差的名称。可以看出与次序无关，即：$$Cov(X,Y)=Cov(Y,X)$$若X，Y独立，则$$Cov(X,Y)=0$$

------
>相关系数公式：$$Corr(X,Y)=Cov(X,Y)/(\sigma_1\sigma_2)$$

### 处理区缺失值的方法
|方法名|方法作用|
|-|-|
|dropa|根据各标签的值中是否存在缺失数据对轴标签进行过滤，可通过阈值调节对缺失值的容忍度|
|fillna|用指定值或插值方法(如ffill或bfill)填充缺失数据|
|isnull|返回一个含有布尔值的对象，这些布尔值表示哪些值是缺失值NA，该对象的类型与源类型一样|
|notnull|isnull的否定式|
其中fillna函数的参数说明表格如下：
|参数|说明|
|-|-|
|value|用于填充缺失值的标量值或字典对象|
|method|插值方式。如果函数调用时未指定其他参数的话，默认为“ffill”|
|axis|待填充的轴，默认axis=0|
|inplace|修改调用者对象而不产生副本|
|limit|(对于前向和后向填充)可以连续填充的最大数量|
