---
layout:     post
title:      "Keras不同层会自动变化维度"
subtitle:   "2018/07/27 变维"
date:       2018-07-27
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180727.jpg?raw=true"
tags:
    - 日记
---


```
    今天周五，仔细研究了一下keras里面一些常用层的作用，比如Embeding层，这个层主要作用是降维，但是实际
使用的时候这一层是直接增加了一个维度，即输入的数据不需要首先进行one-hot编码，这样真的是非常有用，仅仅
输入本身的类型数即可！其次我发现了LSTM层如果需要返回全部序列的内容，需要只定return_sequences为True，否
则仅仅返回最后一个时间步输出的内容，这样就会自定降低一个维度！这两个特殊的功能一定要注意！
```


