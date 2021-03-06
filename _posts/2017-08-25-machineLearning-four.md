---
layout:     post
title:      "机器学习（四）"
subtitle:   "2017/08/25 正则化(Regularization)"
date:       2017-08-25
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170805.jpg?raw=true"
tags:
    - 机器学习
---


### 时间:2017年8月25日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

到现在为止，我们已经学习了几种不同的学习算法，包括线性回归和逻辑回归，它们能够有效地解决许多问题，但是当将它们应用到某些特定的机器学习应用时，会遇到过度拟合(over-fitting)的问题，可能会导致它们效果很差。
如果我们有非常多的特征，我们通过学习得到的假设可能能够非常好地适应训练集（代价函数可能几乎为 0），但是可能会不能推广到新的数据。

就以多项式理解，x 的次数越高，拟合的越好，但相应的预测的能力就可能变差。问题是，如果我们发现了过拟合问题，应该如何处理？

1. 丢弃一些不能帮助我们正确预测的特征。可以是手工选择保留哪些特征，或者使用一些模型选择的算法来帮忙（例如  PCA）
1. 正则化。  保留所有的特征，但是减少参数的大小（magnitude）。

归问题中如果我们的模型是：

$$
h_\theta(x)=\theta_0+\theta_1 x_1+\theta_2 x_2^2+\theta_3 x_3^3+\theta_4 x_4^4
$$

我们可以从之前的事例中看出，正是那些高次项导致了过拟合的产生，所以如果我们能让这些高次项的系数接近于 0 的话，我们就能很好的拟合了。
所以我们要做的就是在一定程度上减小这些参数 θ 的值，这就是正则化的基本方法。我们决定要减少 θ3  和  θ4  的大小，我们要做的便是修改代价函数，在其中  θ3 和  θ4  设置一点惩罚。这样做的话，我们在尝试最小化代价时也需要将这个惩罚纳入考虑中，并最终导致选择较小一些的 θ3  和 θ4。修改后的代价函数如下：

$$
\mathop{minimize}\limits_{\theta}\frac{1}{2m}
\sum_{i=1}^{m}((h_\theta(x^{(i)})-y^{(i)})^2+1000\theta_3^2+10000\theta_4^2)
$$

通过这样的代价函数选择出的 θ3 和 θ4 对预测结果的影响就比之前要小许多。假如我们有非常多的特征，我们并不知道其中哪些特征我们要惩罚，我们将对所有的特征进行惩罚，并且让代价函数最优化的软件来选择这些惩罚的程度。这样的结果是得到了一个较为简单的能防止过拟合问题的假设：

$$
J(\theta)=\frac{1}{2m}
[\sum_{i=1}^{m}((h_\theta(x^{(i)})-y^{(i)})^2+\lambda\sum_{j=1}^{n}\theta_j^2)]
$$

其中 λ 又称为正则化参数（Regularization Parameter）。  注：根据惯例，我们不对 θ0 进行惩罚。
如果选择的正则化参数 λ 过大，则会把所有的参数都最小化了，导致模型变成  hθ(x)=θ0,造成欠拟合。
那为什么增加一项$$\lambda=\sum_{j=1}^{n}\theta_j^2$$可以使 θ 的值减小呢？
因为如果我们令λ的值很大的话，为了使 Cost Function  尽可能的小，所有的 θ 的值（不包括 θ0）都会在一定程度上减小。
但若λ的值太大了，那么 θ（不包括 θ0）都会趋近于 0，这样我们所得到的只能是一条平行于 x 轴的直线。
所以对于正则化，我们要取一个合理的λ的值，这样才能更好的应用正则化。
