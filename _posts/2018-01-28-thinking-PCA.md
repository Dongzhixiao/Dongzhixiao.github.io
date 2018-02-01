---
layout:     post
title:      "周日读论文"
subtitle:   "2018/01/28 思考异常检测方法"
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

该方法详细介绍：

主成分分析首先将m行n列的数据矩阵A输入，中心化数据得到矩阵B，然后解得$$B^TB$$的特征值$$λ_i (i=0,1,…,n)$$，
其中选择方差权重95%，使得

$$(\sum_{i=1}^kλ_i )/(\sum_{j=1}^nλ_j )>95%$$
，

则得到值最高的k个特征向量$$V_1,V_2,…V_k$$。
将这些特征向量组成。因为主成分分析将高维的数据映射到低维空间中，使得每个高维数据到低维空间的距离之和达到最小，
因此大部分数据到低维空间中的距离比较小，所以该距离越大，可以认为原来的数据时间片更可能是异常时间段。
将最高的k个特征向量组成矩阵$$P=[V_1,V_2,…V_k]$$，则该矩阵的正交投影矩阵为$$P(P^T P)^{-1}P^T=PP^T$$。
因此如果原来的向量为y，则该向量到其映射的子空间的欧几里得距离通过计算平方预测误差$$SPE=‖y_a ‖^2$$（矢量$$y_a$$的平方长度）
来形式化(其中$$y_a$$是y到异常子空间$$s_a$$上的投影，并且可以计算为$$y_a=(I-PP^T )y$$，其中$$P=[v_1,v_2,…,v_k]$$
由PCA算法选择的前$$k$$个主成分组成。)

检测规则 ：如果我们标记y是异常的如果：$$SPE=‖y_a ‖^2>Q_\alpha$$   (其中$$Q_\alpha$$表示在$$(1-\alpha)$$置信水平下SPE残差函数的阈值统计量。)


$$
\begin{align}
&\delta^2_\alpha = 
\phi_1[\frac{C_\alpha\sqrt{2\phi_2h^2_0}}{\phi_1}+1+
       \frac{\phi_2h_0(h_0-1)}{\phi_1^2}]^{\frac{1}{h_0}}  \\
其中：&\phi_i=\sum_{j=r+1}^{m}\lambda_j^i   \qquad  (i=1,2,3) \\
      &h_0=1-\frac{2\phi_1\phi_3}{3\phi^2_2}
\end{align}
$$

$$\lambda_j$$是样本数据协方差矩阵第j个主成分投影在子空间的特征值。
$$C_\alpha$$是标准正态分布的$$1-\alpha$$百分位数。

