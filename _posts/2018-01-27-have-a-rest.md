---
layout:     post
title:      "休闲的周六"
subtitle:   "2018/01/27 睡懒觉"
date:       2018-01-27
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180127.jpg?raw=true"
tags:
    - 日记
    - Seaborn
    - Python
---

```
    一到周六，我就完全不像动了，果然自己还是太懒了！因此早晨基本上睡了一上午，起来又玩了会游戏FGO
到中午的时候又不想出门吃饭，然后就点了一个外卖————比萨饼。吃完后又睡了一个午觉。起来时感觉不能再这样
下去了，去办公室学习吧，实在是太颓废了！然下午就总结一下最近学习的Seaborn画图相关的知识吧~
    到了办公室之后，发现曹老师还在加班！曹老师经常周末加班，真是太辛苦了！晚上一直看数据挖掘的书，
感觉非常有意思，最近有时间要仔细看看数据挖掘，数据处理真的是非常有意思的一个工作和研究的内容呀！
```

今天，总结Seaborn画图，其实直接网上搜索Seaborn画图的博客有很多，但是很多都是直接把说明文档罗列一下，
我这里举个例子说明下我使用Seaborn要解决的一个具体问题。
（先列下提纲，以后补充~）
### 问题描述
有100样本，每个样本8个属性的数据，如下图：



我需要画出这些数据。

### Seaborn函数的标签与API规律

查看Seaborn官网的[API](http://seaborn.pydata.org/api.html)，可以看到整个Seaborn画图的实际绘画模块只有
以下七个标签：

- Axis grids
- Categorical plots
- Distribution plots
- Regression plots
- Matrix plots
- Timeseries plots
- Miscellaneous plots

而且每个标签里面的子函数传入参数非常类似，只要掌握一个，其他的直接改下函数名字即可画出来，因此实际
学习Seaborn画图只要看七个函数说明即可！

我画图时主要用到了其中的`Categorical plots`模块下的函数。下面主要以`seaborn.stripplot`函数为例子说明如下：

`seaborn.stripplot(x=None, y=None, hue=None, data=None, order=None, hue_order=None, jitter=False, dodge=False, orient=None, color=None, palette=None, size=5, edgecolor='gray', linewidth=0, ax=None, **kwargs)`

这个函数的最关键的参数就是前4个参数，后面的参数都是微调的问题。

- `x`参数代表绘图的横坐标的数据标签项
- `y`参数代表绘图的纵坐标的数据标签项
- `hue`参数代表同一个x下多标签的数据标签项
- `data`参数代表输入数据，一般格式都是`DataFrame`的形式。

我们可以看一个`Seaborn`自带的例子，将数据读取，然后保存成`excel`文件并打开观察
（这里顺便吐槽一下，DataFrame数据结构实在是太好用、太方便了，可以读取/保存成各种数据格式，
并且操作数据非常方便，<a target="_blank" href="https://dongzhixiao.github.io/2018/02/04/pandas/">下周周末</a>要总结一下！），
代码如下：

```Python
    import seaborn as sns
    import pandas as pd
    import matplotlib.pyplot as plt

    tips = sns.load_dataset("tips")   #读取seaborn自带的一个测试数据
    tips.to_excel('tips.xlsx')   #保存成excel文件，进行观察
```

得到的数据如下图：

![Seaborn数据tips的例子图片](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180127_tips.png?raw=true)


可以看到这是一个典型的打了标签的数据集，这个数据仅仅输入一句代码：

`sns.stripplot(x="sex", y="total_bill", hue="day",data=tips)`

即可得到如下图所示的数据散点图：

![Seaborn例子图片](http://seaborn.pydata.org/_images/seaborn-stripplot-7.png)

我们仔细观察图片，发现该图实际上横坐标是`sex`标签，纵坐标是`total_bill`标签，而图示是根据
`day`标签进行分类绘制的。
但是我们在实际操作数据时，一般原始数据都没有打标签，就像是我一开始给出的数据集那样，每个属性
都是一个数值，但是我们想要看每个属性的分布图，即：x轴上是属性标签，这时我们就需要简单处理
一下我们的数据了。

### 进行数据预处理
对于给出的数据，我们只需要写一个简单的函数即可：

```
    data = [];
    for j in range(df.iloc[:,0].size):
        LineSeries = df.iloc[j]  #得到行的序列
        for k in range(LineSeries.size):
            data.append((LineSeries[k],LineSeries.index[k]))
            #下面一行绝对不要用，DataFrame绝对不要动态加行列！！！！
#            Drawdf.loc[Drawdf.iloc[:,0].size] = [LineSeries[k],LineSeries.index[k],strList[i]]
    DataToDraw = pd.DataFrame(columns=('value','property'),data =data)
    return DataToDraw
```

这样，我们根据函数返回的数据，即可直接带入Seaborn相关的函数进行绘图。

### 调用函数并得到结果

我们可以直接调用多个函数，里面的参数都是一样的

```
    df = pd.read_excel("test.xlsx")
    dfToDraw = generateDrawDF(df)
    plt.figure()
    sns.boxplot(x='property',y='value',data = dfToDraw) #绘制不同属性分布的箱图
    plt.figure()
    sns.violinplot(x='property',y='value',data = dfToDraw) #绘制不同属性小提琴图
```

比如箱图和小提琴图（小提琴是箱线图和密度分布的一个组合），图如下所示：

![绘制箱图的例子](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180127_test_boxplot.png?raw=true)

![绘制小提琴图的例子](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180127_test_violinplot.png?raw=true)

最后，本篇的全部代码在下面这个网页可以下载：

[https://github.com/Dongzhixiao/Python_Exercise/tree/master/seabornTest](https://github.com/Dongzhixiao/Python_Exercise/tree/master/seabornTest)