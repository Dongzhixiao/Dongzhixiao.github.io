---
layout:     post
title:      "用户行为分析论文"
subtitle:   "2018/07/09 MMH"
date:       2018-07-09
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180709.jpg?raw=true"
tags:
    - 日记
    - 论文阅读
---


```
    今天是周一，在没有什么想法的情况下我只能阅读论文，今天阅读的论文名叫《Beyond DCG: User Behavior as a Predictor of a Successful
Search》发表在2010年的WSBM会议上，属于CCFB类里面关于数据挖掘类比较好的会议。本文主要说明传统搜索引擎根据一次查询进行的评价
方式的缺陷：用户一次同样的查询也许需要不同的信息。本文对用户行为建模，并证明用户行为比文档相关性预测的更准确。本文还证明基于用户行为
的序列和时间分布模型比静态模型和基于文档相关模型的预测精度更好。
    文章的思路比较清楚，先将用户动作行为分为特定的6类（6个状态），然后给定搜索目标，从开始搜索到结束搜索的模型中，因为用户操作满足特定时间
进行一个特定动作，即用户行为的操作是时间和动作都离散的，因此如果引入马尔科夫性（即无后效性），则用户从开始到结束的一系列动作可以抽象
为马尔可夫链，论文根据用户操作的结果是否成功该模型建立了两个马尔科夫链模型。然后给定一个用户操作，就可以根据状态连乘计算出成功还是失败的概率，
比较两个概率即可得出是否是一次成功的操作。
    到这里，第一部分就完成了，明天阅读第二部分。
```



