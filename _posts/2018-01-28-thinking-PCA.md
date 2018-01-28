---
layout:     post
title:      "周日读论文"
subtitle:   "2018/01/28 睡懒觉"
date:       2018-01-28
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180128.jpg?raw=true"
tags:
    - 日记
    - 异常检测
    - 数据挖掘
    - Python
---

```
    今天周日，准备读论文，论文是办公室同学小灿的。花了一天的时间，总算把论文读完了，果然我自己水平
还是太低了，只找到了10多个小的问题，而且语法问题一个也没有发现。果然读博士需要好的英语，但是我现在
的英语水平实在是不够用啊，有时间一定要好好学习英语！
```

今天，总结一下异常检测算法。

#### 基于主成分分析的异常检测算法

该算法主要出自于论文：Control procedures for residuals associated with principal component analysis

该方法的公式如下：

$$
\delta^2_\alpha = 
\phi_1[\frac{C_\alpha\sqrt{2\phi_2h^2_0}}{\phi_1}+1+
       \frac{\phi_2h_0(h_0-1)}{\phi_1^2}]^{\frac{1}{h_0}}  \\
其中： h_0=1-\frac{2\phi_1\phi_3}{3\phi^2_2}
$$
