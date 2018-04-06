---
layout:     post
title:      "学习矩阵论的体会"
subtitle:   "2017/04/05 基本思考"
date:       2017-04-05
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170817.jpg?raw=true"
tags:
    - 矩阵学习
    - Latex语法
---

### 时间:2017年4月5日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

记得小时候，看过一部电影，叫做《黑客帝国》，里面说到了矩阵，之后一直感觉矩阵是一个很神奇、很厉害的东西，一直想学习一下，恰好最近比较清闲，于是就用业余时间学习一下矩阵理论。
稍微学习一下后我觉得其中最常用的两个应用就是**求解不相容线性方程组的最小二乘解**和**求解线性微分方程组**，而这两个应用分别需要用到**矩阵函数**和**矩阵的广义逆**的知识，其中这两个知识点又分别需要用到**Jordan标准型**和**矩阵分解**的知识。因此我认为学习矩阵论及其应用可以从两条线入手：

第一条：矩阵分解-->矩阵的广义逆-->求解不相容线性方程组的最小二乘解
第二条：Jordan标准型-->矩阵函数-->求解线性微分方程组

为了能够写出矩阵的公式，我使用了Latex语法，下面是其常用语法的网址：<a target="_blank" href="http://www.mohu.org/info/symbols/symbols.htm">Latex常用语法网址</a>
为了可以根据手写符号得到对应形状的Latex结构，可以使用这个网址：<a target="_blank" href="http://detexify.kirelabs.org/classify.html">Latex手写反求对应符号网址</a>

后期准备按照这两条线，把学习的东西总结一下，以便以后随时复习~

#### Jordan标准型法
$$
g(J_i(\lambda_i))=\left[
\begin{matrix}
g(\lambda_i)&g'(\lambda_i)&\frac{1}{2!}g''(\lambda_i)&\cdots&\frac{g^{(n_i-1)}(\lambda_i)}{(n_i-1)!}\\
&g(\lambda_i)&g'(\lambda_i)&\cdots&\cdots\\
&\ddots&&\ddots&\frac{1}{2!}g''(\lambda_i)\\
&&&\ddots&\\
&&&&g'(\lambda_i)\\
&&&&g(\lambda_i)\\
\end{matrix}
\right]
$$
其中
$$
J_i(\lambda_i)=\left[
\begin{matrix}
\lambda_i&1&&\\
&\ddots&\ddots&\\
&&\ddots&1\\
&&&\lambda_i
\end{matrix}
\right]_{n_i}
$$