---
layout:     post
title:      "机器学习（三）"
subtitle:   "逻辑回归"
date:       2017-08-23
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170805.jpg?raw=true"
tags:
    - 机器学习
---


### 时间:2017年8月23日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

## 分类问题
在分类问题中，你要预测的变量  y 是离散的值，我们将学习一种叫做逻辑回归  (Logistic Regression)  的算法，这是目前最流行使用最广泛的一种学习算法。

在分类问题中，我们尝试预测的是结果是否属于某一个类（例如正确或错误）。分类问题的例子有：判断一封电子邮件是否是垃圾邮件；判断一次金融交易是否是欺诈；之前我们也谈到了肿瘤分类问题的例子，区别一个肿瘤是恶性的还是良性的。

我们从二元的分类问题开始讨论。

我们将因变量(dependant variable)可能属于的两个类分别称为负向类（negative class）和正向类（positive class），则因变量$$y\in\{0,1\}$$，其中  0表示负向类，1表示正向类。

如果我们要用线性回归算法来解决一个分类问题，对于分类，y  取值为  0  或者  1，但如果你使用的是线性回归，那么假设函数的输出值可能远大于  1，或者远小于  0，即使所有训练样本的标签  y 都等于  0  或  1。尽管我们知道标签应该取值 0  或者  1，但是如果算法得到的值远大于 1 或者远小于 0 的话，就会感觉很奇怪。所以我们在接下来的要研究的算法就叫做逻辑回归算法，这个算法的性质是：它的输出值永远在 0  到  1  之间。
顺便说一下，逻辑回归算法是分类算法，我们将它作为分类算法使用。有时候可能因为这个算法的名字中出现了“回归”使你感到困惑，但逻辑回归算法实际上是一种分类算法，它适用于标签  y  取值离散的情况，如：1	0	0	1。

## 假说表示

此前我们说过，希望我们的分类器的输出值在 0 和 1 之间，因此，我们希望想出一个满足某个性质的假设函数，这个性质是它的预测值要在 0 和 1 之间。
根据线性回归模型我们只能预测连续的值，然而对于分类问题，我们需要输出  0  或  1，我们可以预测：
当 hθ 大于等于 0.5 时，预测  y=1。
当 hθ 小于 0.5 时，预测  y=0  

我们引入一个新的模型，逻辑回归，该模型的输出变量范围始终在 0 和 1 之间。  逻辑回归模型的假设是：$$h_\theta(x)=g(\theta^TX)$$
其中：
X  代表特征向量
g  代表逻辑函数（logistic function）是一个常用的逻辑函数为 S 形函数（Sigmoid function），
 公式为： $$g(z)=\frac{1}{1+e^{-z}}$$
图像可以使用python代码画出来：
```
import pandas as pd
import numpy as np
z = np.arange(-10.,10.,0.1)
g_z=pd.Series(1/(1+np.exp(-z)),index=z)
g_z.plot(grid=True)
```
合起来，我们得到逻辑回归模型的假设，对模型的理解：
$$
h_\theta(x)=\frac{1}{1+e^{-\theta^TX}}
$$
hθ(x)的作用是，对于给定的输入变量，根据选择的参数计算输出变量=1   的可能性（estimated probablity）即:
$$
h_\theta(x)=P(y=1|x;\theta)
$$
例如，如果对于给定的 x，通过已经确定的参数计算得出 hθ(x)=0.7，则表示有 70%的几率 y 为正向类，相应地 y 为负向类的几率为  1-0.7=0.3。

## 判定边界
在逻辑回归中，我们预测：
当 hθ 大于等于  0.5  时，预测  y=1
当 hθ 小于  0.5  时，预测  y=0
根据上面绘制出的  S  形函数图像，我们知道当
z=0  时  g(z)=0.5
z>0  时  g(z)>0.5
z<0  时  g(z)<0.5
又  z=θTX，即：
θTX  大于等于  0  时，预测  y=1
θTX  小于  0  时，预测  y=0
现在假设我们有一个模型：
$$
h_\theta(x)=g(\theta_0+\theta_1x_1+\theta_2x_2)
$$
并且参数 θ 是向量[-3  1  1]。  则当-3+x1+x2  大于等于  0，即 x1+x2 大于等于  3  时，模型将预测  y=1。
我们可以绘制直线 x1+x2=3，这条线便是我们模型的分界线，将预测为 1 的区域和预测为  0 的区域分隔开。

假使我们的数据呈现圆形边界的分布情况，怎样的模型才能适合呢？
因为需要用曲线才能分隔  y=0  的区域和  y=1  的区域，我们需要二次方特征：  

假设参数：
$$h_\theta(x)=g(\theta_0+\theta_1x_1+\theta_2x_2+\theta_3x_1^2+\theta_4x_2^2)$$是[-1 0 0 1 1]，

则我们得到的判定边界恰好是圆点在原点且半径为 1 的圆形。我们可以用非常复杂的模型来适应非常复杂形状的判定边界。

## 代价函数
下面我要定义用来拟合参数的优化目标或者叫代价函数，这便是监督学习问题中的逻辑回归模型的拟合问题。
对于线性回归模型，我们定义的代价函数是所有模型误差的平方和。理论上来说，我们也可以对逻辑回归模型沿用这个定义，但是问题在于，当我们将
$$
h_\theta(x)=\frac{1}{1+e^{-\theta^TX}}
$$
带入到这样定义了的代价函数中时，我们得到的代价函数将是一个非凸函数（non-convex function）。这意味着我们的代价函数有许多局部最小值，这将影响梯度下降算法寻找全局最小值。线性回归的代价函数为：
$$
J(\theta)=\frac{1}{2m}
\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})^2
$$
我们重新定义逻辑回归的代价函数为：
$$
J(\theta)=\frac{1}{m}
\sum_{i=1}^{m}Cost(h_\theta(x^{(i)}),y^{(i)})
 $$
其中：
$$
Cost(h_\theta(x),y)=\left\{
\begin{align}
&-log(h_\theta(x))&if\quad y=1\\
&-log(1-h_\theta(x))&if\quad y=0
\end{align}
\right.
$$
这样构建的 Cost(hθ(x),y)函数的特点是：当实际的  y=1  且 hθ 也为  1  时误差为  0，当  y=1，但 hθ 不为 1 时误差随着  hθ 的变小而变大；当实际的  y=0  且 hθ 也为  0  时代价为  0；当  y=0，但  hθ 不为 0 时误差随着  hθ 的变大而变大。
将构建的  Cost(hθ(x),y)简化如下：
$$
Cost(h_\theta(x),y)=-y*log(h_\theta(x))-(1-y)*log(1-h_\theta(x))
$$
带入代价函数得到：
$$
J(\theta)=-\frac{1}{m}\sum_{i=1}^{m}[y^{(i)}*log(h_\theta(x^{(i)}))+(1-y^{(i)})*log(1-h_\theta(x^{(i)}))]
$$
在得到这样一个代价函数以后，我们便可以用梯度下降算法来求得能使代价函数最小的参数了。算法为：
$$
\begin{align}
&重复:\{\\
&\qquad\theta_j := \theta_j-\alpha\frac{\partial}
{\partial\theta_j}J(\theta)\\
&\}
\end{align}
$$
求导后得到：
$$
\begin{align}
&重复:\{\\
&\qquad\theta_j := \theta_j-\alpha
\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})\cdot x^{(i)}_j\\
&\}
\end{align}
$$
我们定义了单训练样本的代价函数，凸性分析的内容是超出这门课的范围的，但是可以证明我们所选的代价值函数会给我们一个凸优化问题。代价函数 J(θ)会是一个凸函数，并且没有局部最优值。
>注：虽然得到的梯度下降算法表面上看上去与线性回归的梯度下降算法一样，但是这里的  hθ(x)=g(θTX)与线性回归中不同，所以实际上是不一样的。另外，在运行梯度下降算法之前，进行特征缩放依旧是非常必要的。

一些梯度下降算法之外的选择：  除了梯度下降算法以外，还有一些常被用来令代价函数最小的算法，这些算法更加复杂和优越，而且通常不需要人工选择学习率，通常比梯度下降算法要更加快速。这些算法有：共轭梯度（Conjugate Gradient），局部优化法(Broyden fletchergoldfarb shann,BFGS)和有限内存局部优化法(LBFGS) fminunc 是  matlab 和 octave  中都带的一个最小值优化函数，使用时我们需要提供代价函数和每个参数的求导，下面是  octave  中使用  fminunc  函数的代码示例：
```
function [jVal, gradient] = costFunction(theta)

jVal = [...code to compute
J(theta)...];

gradient = [...code to compute derivative of J(theta)...];

end

options = optimset('GradObj', 'on', 'MaxIter', '100');

initialTheta = zeros(2,1);

[optTheta, functionVal, exitFlag] = fminunc(@costFunction, initialTheta, options);


```

## 简化的成本函数和梯度下降
将上一节的公式总结一下，下面这就是逻辑回归的代价函数：

$$
J(\theta)=\frac{1}{m}
\sum_{i=1}^{m}Cost(h_\theta(x^{(i)}),y^{(i)})\\
 Cost(h_\theta(x),y)=\left\{
\begin{align}
&-log(h_\theta(x))&if\quad y=1\\
&-log(1-h_\theta(x))&if\quad y=0
\end{align}
\right.\\
注意：y=0或y=1
$$

上面式子合并为：

$$
Cost(h_\theta(x),y)=-y*log(h_\theta(x))-(1-y)*log(1-h_\theta(x))
$$

即逻辑回归的代价函数为：

$$
\begin{align}
J(\theta)&=\frac{1}{m}
\sum_{i=1}^{m}Cost(h_\theta(x^{(i)}),y^{(i)})\\&=-\frac{1}{m}\sum_{i=1}^{m}[y^{(i)}*log(h_\theta(x^{(i)}))+(1-y^{(i)})*log(1-h_\theta(x^{(i)}))]
\end{align}
$$

根据这个代价函数，为了拟合出参数，该怎么做呢？我们要试图找尽量让  J(θ)  取得最小值的参数  θ。

$$
\mathop{minimize}\limits_{\theta}J(\theta)
$$

所以我们想要尽量减小这一项，这将我们将得到某个参数  θ。
如果我们给出一个新的样本，假如某个特征  x，我们可以用拟合训练样本的参数 θ，来输出对假设的预测。
另外，我们假设的输出，实际上就是这个概率值：p(y=1|x;θ)，就是关于  x  以 θ  为参数，y=1  的概率，你可以认为我们的假设就是估计  y=1  的概率，所以，接下来就是弄清楚如何最大限度地最小化代价函数  J(θ)，作为一个关于 θ 的函数，这样我们才能为训练集拟合出参数 θ。
最小化代价函数的方法，是使用梯度下降法(gradient descent)。这是我们的代价函数：

$$
J(\theta)=-\frac{1}{m}\sum_{i=1}^{m}[y^{(i)}*log(h_\theta(x^{(i)}))+(1-y^{(i)})*log(1-h_\theta(x^{(i)}))]
$$

如果我们要最小化这个关于 θ 的函数值，这就是我们通常用的梯度下降法的模板。

$$
\begin{align}
&目标：\mathop{minimize}\limits_{\theta}J(\theta)\\
&重复:\{\\
&\qquad\theta_j := \theta_j-\alpha\frac{\partial}
{\partial\theta_j}J(\theta)\\
&\}
\end{align}
$$

我们要反复更新每个参数，用这个式子来更新，就是用它自己减去学习率  α  乘以后面的微分项。求导后得到：

$$
\begin{align}
&重复:\{\\
&\qquad\theta_j := \theta_j-\alpha
\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})\cdot x^{(i)}_j\\
&\}
\end{align}
$$

如果你计算一下的话，你会得到的是这个式子：

$$
\frac{\partial}{\partial\theta_j}J(\theta)=\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})\cdot x^{(i)}_j
$$
我把它写在这里，将后面这个式子，在 i=1 到 m 上求和，其实就是预测误差乘以$$ x^{(i)}_j$$，所以你把这个偏导数项目$$\frac{\partial}{\partial\theta_j}J(\theta)$$放回到原来式子这里，我们就可以将梯度下降算法写作如下形式：
$$
\theta_j := \theta_j-\alpha
\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})\cdot x^{(i)}_j
$$

所以，如果你有 n 个特征，也就是说：

$$
\theta=\left[\begin{matrix}\theta_0\\\theta_1\\\theta_2\\\dotsi\\\theta_n\end{matrix}\right]
$$

，参数向量θ 包括θ0 θ1 θ2 一直到θn，那么你就需要用这个式子：

$$
\theta_j := \theta_j-\alpha
\sum_{i=1}^{m}(h_\theta(x^{(i)})-y^{(i)})\cdot x^{(i)}_j
$$

来同时更新所有θ的值。

现在，如果你把这个更新规则和我们之前用在线性回归上的进行比较的话，你会惊讶地发现，这个式子正是我们用来做线性回归梯度下降的。
那么，线性回归和逻辑回归是同一个算法吗？要回答这个问题，我们要观察逻辑回归看看发生了哪些变化。实际上，假设的定义发生了变化。
对于线性回归假设函数：$$h_{\theta}(x)=\theta^{\intercal} 	x$$
而现在逻辑函数假设函数：$$h_{\theta}(x)=\frac{1}{1+e^{-z}}$$
因此，即使更新参数的规则看起来基本相同，但由于假设的定义发生了变化，所以逻辑函数的梯度下降，跟线性回归的梯度下降实际上是两个完全不同的东西。当我们在谈论线性回归的梯度下降法时，我们谈到了如何监控梯度下降法以确保其收敛，我通常也把同样的方法用在逻辑回归中，来监测梯度下降，以确保它正常收敛。
当使用梯度下降法来实现逻辑回归时，我们有这些不同的参数 θ，就是 θ0 到 θn，我们需要用这个表达式来更新这些参数。我们还可以使用  for  循环来更新这些参数值，用  for i=1to n，或者  for i=1 to n+1。当然，不用  for  循环也是可以的，理想情况下，我们更提倡使用向量化的实现，可以把所有这些  n  个参数同时更新。
最后还有一点，我们之前在谈线性回归时讲到的特征缩放，我们看到了特征缩放是如何提高梯度下降的收敛速度的，这个特征缩放的方法，也适用于逻辑回归。如果你的特征范围差距很大的话，那么应用特征缩放的方法，同样也可以让逻辑回归中，梯度下降收敛更快。
就是这样，现在你知道如何实现逻辑回归，这是一种非常强大，甚至可能世界上使用最广泛的一种分类算法。