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
---


```
    今天是周周二，继续昨天论文的第二部分。如果昨天的论文仅仅使用了马尔科夫模型，内容并不算多，效果也不算好，文章中在实际中加入了
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
GBDT属于机器学习中的集成学习：






