---
layout:     post
title:      "清明节的第二天"
subtitle:   "2018/04/06 休息"
date:       2018-04-06
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180406.jpg?raw=true"
tags:
    - 日记
    - 凸优化
---

>We will not go quietly into the night! We will not vanish without a fight!
我们不会坐以待毙！我们不会束手就擒！
                                                           ————《独立日》

```
    今天是清明节的第二天。准备复习一下数学知识，主要原因是昨天和杭杰一起讨论数学问题对我启发很大，我
又一次从新思考了数学的相关知识。其实我对数学比较迷糊的是方程和函数的维数区别，微分和积分基本概念的精确
定义。这些其实都是基础概念，我本科学习的时候总是忽略，现在又需要仔细研究了。
    晚上，小明邀请我一起去鱼，一共两锅鱼汤和鱼肉，非常美味！非常感谢小明！
```

下面开始学习凸优化

### 凸函数

#### 基本定义

函数$$f:R^n \to R$$，如果$$domf$$是凸集，且对于任意$$x,y \in domf$$和任意$$0 \leqslant \theta \leqslant 1$$,有

$$f( \theta x+(1- \theta )y) \le \theta f(x)+(1- \theta )f(y)$$

则说明该函数是凸的。

所有仿射函数（包括线性函数）既是凸函数又是凹函数！

#### 一阶条件

函数$$f$$可微(即其梯度$$\bigtriangledown f$$在开集$$domf$$内处处存在)，则函数是凸函数的**充要条件**是:
$$domf$$是凸集且对于任意$$x,y \in domf$$，下式成立

$$f(y) \ge f(x)+ \bigtriangledown f(x)^T(y-x)$$


>这里插入一个小概念，可微和可导的区别，其实微分是关于导数的一个函数，即$$dy=f'(x)dx$$，因此
如果函数可微，必须要求导数$$f'(x)$$在定义域集合上处处存在

该式说明凸函数的一阶泰勒展开实际上就是原函数的一个**全局下估计**。由此可得到凸函数的一个**最重要**的信息：
一个凸函数的局部信息(即它在某点的函数值及其导数)，可以得到一些全局信息(即其全局下估计)

#### 二阶条件

函数$$f$$二阶可微(即对于开集开集$$domf$$内的任意一点，它的Hessian矩阵或者二阶导数$$\bigtriangledown^2 f(x)$$存在)，则
函数是凸函数的**充要条件**是:其Hessian矩阵是半正定矩阵，即对于所有的$$domf$$，有

$$\bigtriangledown^2 f(x)\succeq 0$$

#### 应用--Jensen不等式

Jensen不等式：如果函数$$f$$是凸函数，$$x_1,...,x_k \in domf, \theta_1,...,\theta_k \ge 0$$且
$$\theta_1+ \cdots +\theta_k=1$$，则下式成立：

$$f(\theta_1 x_1+ \cdots + \theta_k x_k) \le theta_1 f(x_1)+ \cdots + \theta_k f(x_k) $$

后面的下次再总结！

