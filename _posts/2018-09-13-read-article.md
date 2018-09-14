---
layout:     post
title:      "深度学习的书籍"
subtitle:   "2018/09/13 交叉熵"
date:       2018-09-13
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180913.jpg?raw=true"
tags:
    - 日记
---


```
    今天，仔细研究了一下明天要将的ppt，发现里面还是有不懂的地方。比如最小化交叉熵训练方法。这个方法
在《深度学习》这边书里面已经解释的很清楚了。
```

考虑一组含有$$m$$个样本的数据集 $$X={ x^{(1)},\dots, x^{(m)} }$$，独立地由未知的真实数据生成分布$$p_{\text{data}}(x)$$生成。

令$$p_{\text{model}}( x; \ )$$是一族由$$\theta$$确定在相同空间上的概率分布。 换言之，$$p_{\text{model}}(x;\theta)$$将任意输入$$x$$映射到实数来估计真实概率$$p_{\text{data}}(x)$$。

对$$\theta$$的最大似然估计被定义为：

$$
\begin{align}
    theta_{\text{ML}} &= \underset{\theta}{argmax} \, p_{\text{model}} (X; \theta),  
        &= \underset{\theta}{argmax} \prod_{i=1}^m p_{\text{model}} (x^{(i)}; \theta).
\end{align}
$$

多个概率的乘积会因很多原因不便于计算。 例如，计算中很可能会出现数值下溢。 为了得到一个便于计算的等价优化问题，我们观察到似然对数不会改变其$argmax$但是将乘积转化成了便于计算的求和形式： 

$$
\begin{equation}
    \theta_{\text{ML}} = \underset{\theta}{argmax} \sum_{i=1}^m \log p_{\text{model}} (x^{(i)}; \theta) .
\end{equation}
$$

因为当我们重新缩放代价函数时$$argmax$$不会改变，我们可以除以$$m$$得到和训练数据经验分布$$\hat{p}{\text{data}}$$相关的期望作为准则： 

$$\begin{equation} \theta_{ML} = \underset{\theta}{argmax} \,E_{x \sim \hat{p}{\text{data}}} \log p{\text{model}} (x; \theta) . \end{equation}$$

一种解释最大似然估计的观点是将它看作最小化训练集上的经验分布$$\hat{p}_{data}$$和模型分布之间的差异，两者之间的差异程度可以通过~KL散度度量。 KL散度被定义为 
$$\begin{equation} D_{KL}(\hat{p}_{\text{data}} | p{\text{model}}) = E_{x \sim \hat{p}{\text{data}}} [ \log \hat{p}{\text{data}}(x) - \log p_{model}(x) ] . \end{equation}$$ 
左边一项仅涉及到数据生成过程，和模型无关。 这意味着当我们训练模型最小化~KL散度时，我们只需要最小化 
$$\begin{equation} -E_{x \sim \hat{p}{\text{data}}} [ \log p{\text{model}}(x) ] , \end{equation} $$
当然，这和\eqn中最大化是相同的。

最小化~KL散度其实就是在最小化分布之间的交叉熵。 许多作者使用术语”交叉熵”特定表示伯努利或softmax分布的负对数似然，但那是用词不当的。 
任何一个由负对数似然组成的损失都是定义在训练集上的经验分布和定义在模型上的概率分布之间的交叉熵。 例如，均方误差是经验分布和高斯模型之间的交叉熵。

我们可以将最大似然看作是使模型分布尽可能地和经验分布$$\hat{p}_{data}$$相匹配的尝试。 理想情况下，我们希望匹配真实的数据生成分布$$p_{data}$$，但我们没法直接知道这个分布。

虽然最优$$\theta$$在最大化似然或是最小化~KL散度时是相同的，但目标函数值是不一样的。 
在软件中，我们通常将两者都称为最小化代价函数。 因此最大化似然变成了最小化负对数似然（NLL)，或者等价的是最小化交叉熵。 将最大化似然看作最小化~KL散度的视角在这个情况下是有帮助的，因为已知~KL散度最小值是零。 
当$$x$$取实数时，负对数似然可以是负值。


