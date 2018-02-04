---
layout:     post
title:      "周六总结"
subtitle:   "2018/02/03 关联分析"
date:       2018-02-03
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180203.jpg?raw=true"
tags:
    - 日记
    - 关联分析
    - 数据挖掘
    - Python
---

```
    今天周六，我一早来到了实验室，发现昨天晚上清理的东西还没有完成，那就继续清理，过了一会，曹老师
又过来加班了，我看了一会关联规则，就快到中午了。刘伯禹让一位学生来我这领取一个电脑检查的工具，我一看，
是李瑞林师姐，师姐显然不认识我，还叫我王晓东老师。呵呵，我坐在小办公室确实很容易被叫做老师。
    中午，因为和师兄有约，所有我必须早点走。师兄今天下午要坐飞机飞往美国，所以临别想和师兄吃一顿饭。
顺便祝愿师兄在美国一帆风顺！一起吃饭的还有另一位准备出国的同学，名叫李春。跟他聊天，发现他认识实验室的
同学小灿，原来他们以前都是中央名族大学毕业的，只不过李春师兄硕士毕业时，小灿刚本科毕业。而现在李春师兄
在读博二，小灿已经博一了，这证明硕博连读确实比较节约时间。
    下午，送走师兄后，我来到小明的实验室，和他一起讨论关联分析问题，顺便把关联分析给总结了一下，加深印象。
    晚上，准备总结一下异常检测的问题，结果感觉非常困，就早点睡觉吧~
```


## 关联分析

### 选择函数包

关联分析属于数据挖掘的一大类。我发现的python语言实现的包有两个：

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
进入函数说明文档[页面](http://orange3-associate.readthedocs.io/en/latest/scripting.html)

首先看第一个函数`fpgrowth.frequent_itemsets(X, min_support=0.2)`
这个函数代表计算给定支持度下得到的频繁项集，返回的是一个频繁项集的列表生成器。

- X代表输入的数组类型的数据(list or numpy.ndarray or scipy.sparse.spmatrix or iterator))
- min_support代表关联规则设置的置信度，默认为0.2

>注意：该函数返回的列表生成器只能使用一次，我在这里调试时出了好多次错误，每次用完后我调用这个生成器，
然后总是得到空集，所以干脆直接返回值包上字典函数`dict`并返回给变量`itemsets`，这样可以多次使用

预处理做好了，这里分析仅仅一行代码即可：

```
itemsets = dict(oaf.frequent_itemsets(listToAnalysis, .02)) #这里设置支持度
```

返回的`itemsets`就是频繁项集，这时可以调用第二个函数`fpgrowth.association_rules(itemsets, min_confidence, itemset=None)`
这个函数代表在给定置信度和第一步得到的频繁项集的情况下得到关联规则

- itemsets代表第一个函数返回的字典数据集
- min_confidence代表关联规则设置的支持度
- itemset代表仅仅生成该频繁项集下的规则，这个项集必须是itemsets这个字典里的一个键，数据结构格式是`frozenset`，例如`frozenset({'TimeInterval_100_200'})`

>注意：该函数返回的列表生成器只能使用一次，我在这里调试时出了好多次错误，每次用完后我调用这个生成器，
然后总是得到空集，所以干脆直接返回值包上列表函数`list`并返回给变量`itemsets`，这样可以多次使用

关联规则生成仅仅两行代码即可：

```
rules = oaf.association_rules(itemsets, .5)   #这里设置置信度
rules = list(rules)
```

`rules`的结果是元祖，每一个值都是`frozenset,frozenset,suport,confidence`形式，例如：
`(frozenset({'AT_地球科学', 'C_IGSNRR,CAS', 'TI_100_200'}), frozenset({'RN_0_20'}), 7, 1.0)`
我为了使结果更有利于观察，自己加了个函数得到可打印的便于观察的规则，代码如下：

```
def dealResult(rules):
    returnRules = []
    for i in rules:
        temStr = '';
        for j in i[0]:   #处理第一个frozenset
            temStr = temStr+j+'&'
        temStr = temStr[:-1]
        temStr = temStr + ' ==> '
        for j in i[1]:
            temStr = temStr+j+'&'
        temStr = temStr[:-1]
        temStr = temStr + ';' +'\t'+str(i[2])+ ';' +'\t'+str(i[3])+ ';' +'\t'+str(i[4])+ ';' +'\t'+str(i[5])+ ';' +'\t'+str(i[6])+ ';' +'\t'+str(i[7])
#        print(temStr)
        returnRules.append(temStr)
    return returnRules

printRules = dealRules(rules)
```

这里可以打印`printRules`观察结果

返回的规则`rules`就是规则列表，即可调用第三个函数`fpgrowth.rules_stats(rules, itemsets, n_examples)`
这个函数代表在给规则列表和频繁项集和总样例数目的情况下得到关联规则相关评价结果

- rules代表第二个函数得到的规则列表
- itemsets代表第一个函数返回的字典数据集
- n_examples代表实例的总数

代码如下：
```
result = list(oaf.rules_stats(rules, itemsets, len(listToAnalysis)))   #下面这个函数改变了rules和itemsets，把rules和itemsets置空！
```

>注意，使用完第三个函数后，输入的itemsetes和rules都空了！

为了使得结果便于观察和保存，我最近实现了一个函数，将结果清晰的写到一个`excel`表格里面，代码如下：

```
def ResultDFToSave(rules):   #根据Qrange3关联分析生成的规则得到并返回对于的DataFrame数据结构的函数
    returnRules = []
    for i in rules:
        temList = []
        temStr = '';
        for j in i[0]:   #处理第一个frozenset
            temStr = temStr + str(j) + '&'
        temStr = temStr[:-1]
        temStr = temStr + ' ==> '
        for j in i[1]:
            temStr = temStr + str(j) + '&'
        temStr = temStr[:-1]
        temList.append(temStr); temList.append(i[2]); temList.append(i[3]); temList.append(i[4])
        temList.append(i[5]); temList.append(i[6]); temList.append(i[7])
        returnRules.append(temList)
    return pd.DataFrame(returnRules,columns=('规则','项集出现数目','置信度','覆盖度','力度','提升度','利用度'))
 

dfToSave = ResultDFToSave(result)
dfToSave.to_excel('regular.xlsx')
```

### 置信度和支持度的选择

置信度和支持度一般都是根据几个数据得到的规则数目自己选择的，我编写了一个程序，可以得到不同置信度和支持度下生成规则的数目，
用来决定具体选择多少，代码如下：

```
#######################################################下面是根据不同置信度和关联度得到关联规则数目
    listTable = []
    supportRate = 0.1
    confidenceRate = 0.1
    for i in range(9):
        support = supportRate*(i+1)
        listS = []
        for j in range(9):
            confidence = confidenceRate*(j+1)
            itemsets = dict(oaf.frequent_itemsets(listToAnalysis, support))
            rules = list(oaf.association_rules(itemsets, confidence))
            listS.append(len(rules))
        listTable.append(listS)    
    dfList = pd.DataFrame(listTable,index = [supportRate*(i+1) for i in range(9)],columns=[confidenceRate*(i+1) for i in range(9)])
    dfList.to_excel('regularNum.xlsx')
```

最后，本篇的全部代码在下面这个网页可以下载：

[https://github.com/Dongzhixiao/Python_Exercise/tree/master/associate](https://github.com/Dongzhixiao/Python_Exercise/tree/master/associate)





