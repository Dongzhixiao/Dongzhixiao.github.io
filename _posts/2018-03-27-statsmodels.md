---
layout:     post
title:      "学习统计相关操作"
subtitle:   "2018/03/27 总结"
date:       2018-03-27
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180327.jpg?raw=true"
tags:
    - 日记
    - statsmodels
---

```
    今天终于感觉头脑清楚一点了，赶快思考一下如何分析Messages类型的日志！看了一会，小明给我发了条消息，
说有我的快递，我非常惊奇，因为最近我并没有在网上买过任何东西呀。后来小明告诉我，原来上次的竞赛的奖了，
小明送我一个书包表示感谢。其实我觉得我应该对小明表示感谢，因为他让我参加了这样一个有意思的比赛，让我
学到了很多东西。收到了小明的礼物，我非常高兴！
```

### Statsmodels的学习

感觉`dmatrices`是一个很好用的预处理函数，功能如下

```
Notice that dmatrices has
注意到dmatrices具有以下几个功能
    split the categorical Region variable into a set of indicator variables.
    将分类区域变量分解为一组指示器变量。
    added a constant to the exogenous regressors matrix.
    增加了一个常数给外源的回归矩阵。
    returned pandas DataFrames instead of simple numpy arrays. This is useful because DataFrames allow statsmodels to carry-over meta-data (e.g. variable names) when reporting results.
    返回的熊猫是DataFrames，而不是简单的numpy数组。这是有用的，因为DataFrames允许statsmo模型在报告结果时对元数据（例如变量名）进行处理。
```

得到处理好的数据后，我们即可进行一些统计分析，最常用的就是

```
Fitting a model in statsmodels typically involves 3 easy steps:
在statsmomodel中安装一个模型通常需要3个简单的步骤：
    Use the model class to describe the model
    使用模型类来描述模型
    Fit the model using a class method
    使用类方法来匹配模型
    Inspect the results using a summary method
    使用摘要方法检查结果
```






