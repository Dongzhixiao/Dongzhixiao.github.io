---
layout:     post
title:      "实践精度图像绘制"
subtitle:   "2018/07/18 代码实践"
date:       2018-07-18
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180718.jpg?raw=true"
tags:
    - 日记
---


```
    今天，我继续研究LSTM神经网络，通过实践，把用户行为的数据转换成为神经网络的输入，最终成功的进行了预测。
然后我发现需要测试一下数据的精度，同时，我希望能够通过tensorboard将结果绘制出来，因此我仔细研究了一下如何
使用相关数据，最后得到了几个输出的接口。最终的实践显示效果不错。不过我无法绘制出召回率和ROC曲线，
原因是我的样本中没有正确和错误的概念。仅仅是进行序列预测。今天预测的时候从新进行了样本的分离，70%训练集，
30%测试集。明天准备给老师说下结果。
```




