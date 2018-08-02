---
layout:     post
title:      "终于实现了attention机制"
subtitle:   "2018/07/31 注意力"
date:       2018-07-31
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180731.jpg?raw=true"
tags:
    - 日记
---


```
    今天一早，我就激动的将师兄说的方法进行运用，在构造mask函数的时候，直接用下面这个语句：
self.mask = K.variable(np.triu(np.ones((input_dim,input_dim))))来实现，这样就得到一个非常好的mask矩阵
返回的时候非常顺利，然后我就进行测试，发现精度提高了一个百分点，真的是非常高兴！attention机制确实
非常厉害！但是在读取数据的时候遇到了问题，因为自定义层keras不知道怎么读取，最后通过研究，发现读取
自定义层的时候需要加上with keras.utils.CustomObjectScope({'Dense_New': Dense_New}):这句话，这样
使得keras可以认识自定义层，同时自定义层的get_config函数需要写入自己设定的所有参赛，让其能够顺利
读取！
    下午，我也迫不及待的研究了一下attention绘图的相关操作，最后总要将attention的权重成功绘制出
了图像，感觉非常满意。之后，杭杰突然联系我，说他今天有空，于是我邀请他过来和我一起吃饭，我们
互相聊聊最近的进展，感到非常有趣！
```


