---
layout:     post
title:      "周六总结"
subtitle:   "2018/02/03 关联分析"
date:       2018-02-02
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180203.jpg?raw=true"
tags:
    - 日记
    - 关联分析
    - 数据挖掘
    - Python
---

```
    今天周六，我一早来到了实验室，发现昨天晚上清理的东西还没有完成
```


## 关联分析

### 选择函数包

关联分析属于数据挖掘的一大类。我发现的包有两个：

- [pymining](https://github.com/bartdag/pymining "pymining网址")：根据Apriori算法进行关联规则挖掘
- [Orange3的关联规则库](https://github.com/biolab/orange3-associate)：根据FP-growth算法进行关联规则挖掘

经过分析，我决定使用`Oranges`进行关联规则的实现，原因如下：

- `FP-growth`算法比`Apriori`算法时间复杂度低
- `Orange3`是一整套数据挖掘工具包，学习后可以熟悉相关操作，进行其他的数据挖掘算法的研究
- `pymining`不再维护，`Orange3`仍然是一个非常活跃的包，更新频繁
- `Orange3`实现的结果比较多，除了规则外，还能够计算出评价结果的相关数据

>注意：`Orange3`的关联分析模块安装时需要在`Anaconda`的命令行窗口中输入以下命令`pip install orange3-associate`

### 数据输入

对于使用函数包来说，我们不用管函数实现的方法，只有研究数据输入的格式即可。

`Orange3`的关联规则输入支持两种形式：

- 布尔类型
- 字符串类型

#### 对于布尔类型

每一个行向量代表一个属性是否存在的数据结构

```
>>> X
array([[False,  True, ...,  True, False],
       [False,  True, ...,  True, False],
       [ True, False, ..., False, False],
       ...,
       [False,  True, ...,  True, False],
       [ True, False, ..., False, False],
       [ True, False, ..., False, False]], dtype=bool)
```

比如上面的数据`X`，注意这个`array`(属于numpy里面的多维数组)。类型一定是`bool`才行。
这个二维数组每一个行的维度都是一样的，这样得到的规则结果就是纯粹数组直接的关联
规则，我们要自己讲对于规则的数字和属性名称对于起来。
比如结果可能是这样：

```
>>> rules
    [(frozenset({17, 2, 19, 20, 7}), frozenset({41}), 41, 1.0),
     (frozenset({17, 2, 19, 7}), frozenset({41}), 41, 1.0),
     ...
     (frozenset({20, 7}), frozenset({41}), 41, 1.0),
     (frozenset({7}), frozenset({41}), 41, 1.0)]
```

#### 对于字符串类型
这里我们输入就不要求每个数据的维度相同了，我们仅仅把出现的属性字符给输入进去即可。
比如下面的例子，我把上次爬虫得到的数据进行关联规则分析。

输入数据的excel表格为`NS_new.xls`,表格内容截图如下：

![Nature爬虫得到的数据](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180203_test.png?raw=true)

我们可以看到，我们一共有`ArticleTag`、`ReceivedTime`、`PublishedTime`、`TimeInterval`、`ReferencesNum`、`Country`这
6个属性。其中`TimeInterval`属性为空，用来填写`PublishedTime`和`ReceivedTime`属性之间的时间差的数据。因为我们想要分析
时间差和其他属性之间的关联关系，而不是“接收时间”与“发表时间”与其他属性之间的关联关系。

时间差需要操作两个列属性，而且两个列属性之间的减法是时间日期相关的操作，涉及到每个月份不一样，最好能之间调用系统的函数。
这样就需要自己考虑年月之间的详细变化了，下面之间使用`Datafram`里面的`apply`函数进行列的操作，具体`apply`函数的总结放到
[明天]()。

```
import pandas as pd
import datetime   #用来计算日期差的包
import orangecontrib.associate.fpgrowth as oaf  #进行关联规则分析的包

def getInterval(arrLike):  #用来计算日期间隔天数的调用的函数
    PublishedTime = arrLike['PublishedTime']
    ReceivedTime = arrLike['ReceivedTime']
#    print(PublishedTime.strip(),ReceivedTime.strip())
    days = dataInterval(PublishedTime.strip(),ReceivedTime.strip())  #注意去掉两端空白
    return days

if __name__ == '__main__':    
    fileName = "NS_new.xls";
    df = pd.read_excel(fileName) 
    df['TimeInterval'] = df.apply(getInterval , axis = 1)
```

计算完成后，所有需要分析的数据都在`df`里面了。

### 对输入数据进行编码

有了数据之后，我们先要对原始数据进行编码。
`ArticleTag`属性，其本身的字符串就可代表对应的类别，因此不用处理。
`ReceivedTime`、`PublishedTime`属性是生产`TimeInterval`属性值得因此，不用处理。
`TimeInterval`属性根据具体计算结果确定，经过观察，使用如下规则：

- 超过三百天为：`TimeInterval_300`，
- 二百到三百天为：`TimeInterval_200_300`
- 一百到二百天为：`TimeInterval_100_200`
- 零到一百天为：`TimeInterval_0_100`

`ReferencesNum`的编码和`TimeInterval`类似。
`Country`直接使用国家字符串即可。

我们需要将数据放入到一个列表中用来分析，每个列表的一行代表一个数据输出的项目，设该列表名字是`listToAnalysis`，
则生成该列表的代码如下：

```
listToAnalysis = []
    listToStore = []
    for i in range(df.iloc[:,0].size):             #df.iloc[:,0].size
        #处理ArticleTag段位
        s = df.iloc[i]['ArticleTag']
        #s = re.sub('\s','_',s.split('/')[1].strip())
        s = s.strip()
        s = 'ArticleTag_'+s
        listToStore.append(s)
        #处理TimeInterval段位
        s = df.iloc[i]['TimeInterval']
        if s > 300:
            s = 'TimeInterval_'+'300'
        elif s > 200 and s <= 300:
            s = 'TimeInterval_'+'200_300'
        elif s > 100 and s <= 200:
            s = 'TimeInterval_'+'100_200'
        elif s <= 100:
            s = 'TimeInterval_'+'0_100'
        listToStore.append(s)
        #处理ReferencesNumber段位
        s = df.iloc[i]['ReferencesNumber']
        if s > 80:
            s = 'ReferencesNumber'+'80'
        elif s > 60 and s <= 80:
            s = 'ReferencesNumber'+'60_80'
        elif s > 40 and s <= 60:
            s = 'ReferencesNumber'+'40_60'
        elif s > 20 and s <= 40:
            s = 'ReferencesNumber'+'20_40'
        elif s > 0 and s <= 20:
            s = 'ReferencesNumber'+'0_20'
        listToStore.append(s)
        #处理Country段位
        s = df.iloc[i]['Country']
        s = 'Country_'+s.strip()
        listToStore.append(s)
        #print(listToStore)
        listToAnalysis.append(listToStore.copy())
        listToStore.clear()
```

### 使用关联相关的函数进行数据处理

将输入数据的格式完成之后，就可以使用关联规则函数进行数据挖掘了。

首先第一个函数``
