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
到中午的时候又不想出门吃饭，然后就点了一个外卖————比萨饼。吃完后又睡了一个午觉。起来时感觉不能在这样
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

我画图时主要用到了其中的`Categorical plots`模块下的函数。下面主要说明下


### 进行数据预处理


### 调用函数并得到结果


