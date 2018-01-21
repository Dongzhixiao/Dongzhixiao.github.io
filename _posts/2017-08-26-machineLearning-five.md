---
layout:     post
title:      "机器学习（五）"
subtitle:   "2017/08/26 神经网络的学习(Neural Networks: Learning)"
date:       2017-08-26
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170805.jpg?raw=true"
tags:
    - 机器学习
---


### 时间:2017年8月26日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

##代价函数
首先引入一些便于稍后讨论的新标记方法：
假设神经网络的训练样本有 m 个，每个包含一组输入 x 和一组输出信号 y，L 表示神经网络层数，Sl 表示每层的  neuron  个数(SL  表示输出层神经元个数)，SL  代表最后一层中处理单元的个数。
将神经网络的分类定义为两种情况：二类分类和多类分类，
二类分类：SL=1, y=0 or 1 表示哪一类；
K 类分类：SL=K, yi = 1 表示分到第 i 类；（K>2）
我们回顾逻辑回归问题中我们的代价函数为：

$$
J(\theta)=-\frac{1}{m}\sum_{i=1}^{m}[y^{(i)}*log(h_\theta(x^{(i)}))+(1-y^{(i)})*log(1-h_\theta(x^{(i)}))]+\frac{\lambda}{2m}\sum_{j=1}^{n}\theta^2_j
$$

在逻辑回归中，我们只有一个输出变量，又称标量（scalar），也只有一个因变量 y，但是在神经网络中，我们可以有很多输出变量，我们的  hθ(x)是一个维度为  K  的向量，并且我们训练集中的因变量也是同样维度的一个向量，因此我们的代价函数会比逻辑回归更加复杂一些，为：

$$
\begin{align}
&神经网络：\\
&\qquad h_\theta(x)\in R^k\quad (h_\theta(x))_i=i^{th}\quad输出：\\
&J(\theta)=-\frac{1}{m}\sum_{i=1}^{m}\sum_{k=1}^{k}[y_k^{(i)}log(h_\theta(x^{(i)}))_k+(1-y_k^{(i)})log(1-h_\theta(x^{(i)}))_k]+\frac{\lambda}{2m}\sum_{l=1}^{L-1}\sum_{i=1}^{sl}\sum_{j=1}^{sl+1}(\theta^{(l)}_{ji})^2
\end{align}
$$

这个看起来复杂很多的代价函数背后的思想还是一样的，我们希望通过代价函数来观察算法预测的结果与真实情况的误差有多大，唯一不同的是，对于每一行特征，我们都会给出K 个预测，基本上我们可以利用循环，对每一行特征都预测 K 个不同结果，然后在利用循环在 K 个预测中选择可能性最高的一个，将其与 y 中的实际数据进行比较。
归一化的那一项只是排除了每一层 θ0 后，每一层的 θ  矩阵的和。最里层的循环 j  循环所有的行（由  sl +1  层的激活单元数决定），循环 i 则循环所有的列，由该层（sl 层）的激活单元数所决定。即：hθ(x)与真实值之间的距离为每个样本-每个类输出的加和，对参数进行egularization 的 bias 项处理所有参数的平方和。

##反向传播算法
网上有很好的证明过程，请看网址：http://ufldl.stanford.edu/wiki/index.php/%E5%8F%8D%E5%90%91%E4%BC%A0%E5%AF%BC%E7%AE%97%E6%B3%95
还有一篇更详细的：
http://blog.csdn.net/itplus/article/details/11022243

##小结
小结一下使用神经网络时的步骤：

网络结构：第一件要做的事是选择网络结构，即决定选择多少层以及决定每层分别有多少个单元。

第一层的单元数即我们训练集的特征数量。

最后一层的单元数是我们训练集的结果的类的数量。

如果隐藏层数大于 1，确保每个隐藏层的单元个数相同，通常情况下隐藏层单元的个数越多越好。

我们真正要决定的是隐藏层的层数和每个中间层的单元数。

训练神经网络：

1.	参数的随机初始化
2.	利用正向传播方法计算所有的 hθ(x)
3.	编写计算代价函数 J 的代码
4.	利用反向传播方法计算所有偏导数
5.	利用数值检验方法检验这些偏导数
6.	使用优化算法来最小化代价函数
