---
layout:     post
title:      "继续昨天论文阅读"
subtitle:   "2018/07/10 MMH"
date:       2018-07-10
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180710.jpg?raw=true"
tags:
    - 日记
    - 论文阅读
    - XGBoost
---


```
    今天是周二，继续昨天论文的第二部分。如果昨天的论文仅仅使用了马尔科夫模型，内容并不算多，效果也不算好，文章中在实际中加入了
时间的因素，因为每个动作都对于这一个时间，因此动作直接的时间大小也对模型起着很大的作用。在对时间进行建模时，作者假设每个动作直接
的时间分布满足gamma分布。因此在给定多个样本的情况下，就可以根据最大似然估计得到时间分布的参数估计值。这样就可以增加时间的因素进入
到HMM里面。
    在实验的时候，作者对比了普通机器学习算法和关联预测方法。
    （1）和机器学习方法对比时使用的是Gradient Boosted Decision
Trees (GBDT)梯度增强决策树。使用一系列特征，并且其中时间特征着重强调了一下，使用的是最大、最小和平均时间。同时也测试了增加HMM，这里
HMM的计算结果简单的当做一个值特征。
    （2）和关联预测方法对比时使用的是论文《How well does result relevance predict session satisfaction?》里面的关联方法和论文《Cumulated 
gain-based evauation of IR techniques》里面的DCG方法。
    最后，作者增加了时间分布的结果，文中没有明确写出，增加时间我的理解是HMM的计算时在每两个状态之间直接乘以时间分布对应的概率。
```

今天在阅读论文的时候用到了机器学习中的GBDT方法，这里简单介绍一下：

## GBDT所属

GBDT属于机器学习中的集成学习。

## GBDT简介

gbdt全称梯度下降树，在传统机器学习算法里面是对真实分布拟合的最好的几种算法之一，
在前几年深度学习还没有大行其道之前，gbdt在各种竞赛是大放异彩。原因大概有几个，
一是效果确实挺不错。
二是即可以用于分类也可以用于回归。
三是可以筛选特征。
这三点实在是太吸引人了！

## GBDT使用

现在最常用的GBDT库是<a target="_blank" href="http://xgboost.readthedocs.io/en/latest/">xgboost</a>
直接看python的使用方法，发现一个最简单的例子：

```
import xgboost as xgb
# read in data
dtrain = xgb.DMatrix('demo/data/agaricus.txt.train')
dtest = xgb.DMatrix('demo/data/agaricus.txt.test')
# specify parameters via map
param = {'max_depth':2, 'eta':1, 'silent':1, 'objective':'binary:logistic' }
num_round = 2
bst = xgb.train(param, dtrain, num_round)
# make prediction
preds = bst.predict(dtest)
```

这个例子显示其使用方法非常简单，这个库所包含的所有方法在<a target="_blank" href="http://xgboost.readthedocs.io/en/latest/parameter.html">参数页面</a>
查看，这里我们可以看到General Parameters里面的第一个参数的解释：

```
booster [default= gbtree ]
Which booster to use. Can be gbtree, gblinear or dart; 
gbtree and dart use tree based models while gblinear uses linear functions.
```

可以看出默认使用的就是gbtree方法，即GBDT方法进行计算。

