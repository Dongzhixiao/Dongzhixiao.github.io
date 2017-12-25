---
layout:     post
title:      "矩阵基础(一)"
subtitle:   "矩阵相乘  线性方程组的矩阵理解  用高斯消元法解线性方程组"
date:       2017-08-17
author:     "WangXiaoDong"
header-img: "img/20170817.jpg"
tags:
    - 矩阵学习
---


### 时间:2017年8月17日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

## 矩阵相乘
### 矩阵和列向量相乘的理解
矩阵乘以列向量可以看做矩阵列向量的线性组合，比如：
$$
\left[\begin{matrix}1&2&3\\4&5&6\\7&8&9\\\end{matrix}\right]
*
\left[\begin{matrix}a\\b\\c\end{matrix}\right]
$$
可以看做矩阵三个列向量关于右边列向量的每个元素的线性组合，即：
$$
\left[\begin{matrix}1\\2\\3\\\end{matrix}\right]*a+
\left[\begin{matrix}4\\5\\6\\\end{matrix}\right]*b+
\left[\begin{matrix}7\\8\\9\\\end{matrix}\right]*c=
\left[\begin{matrix}a+4b+7c\\2a+5b+8c\\3a+6b+9c\\\end{matrix}\right]
$$
### 行向量和矩阵相乘的理解
行向量乘以矩阵可以看做矩阵行向量的线性组合，比如：
$$
\left[\begin{matrix}a&b&c\end{matrix}\right]
*
\left[\begin{matrix}1&2&3\\4&5&6\\7&8&9\\\end{matrix}\right]
$$
可以看做矩阵三个行向量关于左边行向量的每个元素的线性组合，即：
$$
\left[\begin{matrix}1&2&3\\\end{matrix}\right]*a+
\left[\begin{matrix}4&5&6\\\end{matrix}\right]*b+
\left[\begin{matrix}7&8&9\\\end{matrix}\right]*c=
\left[\begin{matrix}a+4b+7c&2a+5b+8c&3a+6b+9c\\\end{matrix}\right]
$$
### 矩阵和矩阵相乘的理解
通过前面的理解，矩阵和矩阵相乘既可以看做行向量的线性组合，也可以看做列向量的线性组合，比如矩阵：
$$
A_{op}*B_{pq}=C_{oq}
$$
这个矩阵的结果可以理解为一下两种情况：

- 矩阵C的第n列等于矩阵A的每个列向量关于矩阵B的第n列向量的每个元素的线性组合
- 矩阵C的第n行等于矩阵B的每个行向量关于矩阵A的第n行向量的每个元素的线性组合

## 线性方程组的矩阵理解
有了对矩阵的理解，我们来看线性方程组：
$$
\begin{equation}
\left\{\begin{aligned}
x+4y+7z=12\\2x+5y+8z=15\\3x+6y+9z=18
\end{aligned}
\right.\end{equation}
$$
显然，根据**矩阵和列向量相乘的理解**，我们可以把这个方程组写成如下形式：
$$
\begin{equation}
\left\{\begin{aligned}
\left[\begin{matrix}1&2&3\\4&5&6\\7&8&9\\\end{matrix}\right]
*
\left[\begin{matrix}x\\y\\z\end{matrix}\right]=
\left[\begin{matrix}12\\15\\18\end{matrix}\right]
\end{aligned}
\right.\end{equation}
$$
这样，解线性方程组就可以理解为找打一个方程系数矩阵列向量的线性组合，使得结果等于右边的列向量。
## 用高斯消元法解线性方程组
高斯消元法就是使得左边的方程的系数矩阵变为LU矩阵的U型矩阵，即上三角矩阵。
变换方式，如何将系数矩阵变为上三角阵，基本方式是一列一列的变。首先考虑把第一列的主元外的元素变为0，比如矩阵：
$$
\left[\begin{matrix}1&2&3\\4&5&6\\7&8&9\\\end{matrix}\right]
$$
先考虑将第二行，第一列的元素4变为零。后面的变换都为行变换，因此要结合前面对“矩阵和矩阵相乘的理解的行向量的线性组合”的观点。即满足：
$$
E_{12}*\left[\begin{matrix}1&2&3\\4&5&6\\7&8&9\\\end{matrix}\right]=
\left[\begin{matrix}1&2&3\\0&?&?\\7&8&9\\\end{matrix}\right]
$$
如何求得初等矩阵$$E_{12}$$
>注：12代表使用第一行的向量化简第二行的向量

很明显结果的第一行和第三行没有变化，因此初等矩阵$$E_{12}$$的第一行为：$$\left[\begin{matrix}1&0&0\end{matrix}\right]$$
因为：
$$
\left[\begin{matrix}1&0&0\end{matrix}\right]
*
\left[\begin{matrix}1&2&3\\4&5&6\\7&8&9\\\end{matrix}\right]=\left[\begin{matrix}1&2&3\\\end{matrix}\right]*1+
\left[\begin{matrix}4&5&6\\\end{matrix}\right]*0+
\left[\begin{matrix}7&8&9\\\end{matrix}\right]*0=
\left[\begin{matrix}1&2&3\\\end{matrix}\right]
$$
同理第三行为：$$\left[\begin{matrix}0&0&1\end{matrix}\right]$$
第二行需要将4化为0，这样需要第一行乘以(-4)加到第二行上，初等矩阵$$E_{12}$$的第二行为：$$\left[\begin{matrix}-4&1&0\end{matrix}\right]$$这样结果可以写成：
$$
\left[\begin{matrix}1&0&0\\-4&1&0\\0&0&1\\\end{matrix}\right]*\left[\begin{matrix}1&2&3\\4&5&6\\7&8&9\\\end{matrix}\right]=
\left[\begin{matrix}1&2&3\\0&-3&-6\\7&8&9\\\end{matrix}\right]
$$
其中：
$$
E_{12}=\left[\begin{matrix}1&0&0\\-4&1&0\\0&0&1\\\end{matrix}\right]
$$
这就是通过一次初等变换后系数矩阵的结果。
同理，第二次初等变换为：
$$
\left[\begin{matrix}1&0&0\\0&1&0\\-7&0&1\\\end{matrix}\right]*\left[\begin{matrix}1&2&3\\0&-3&-6\\7&8&9\\\end{matrix}\right]=
\left[\begin{matrix}1&2&3\\0&-3&-6\\0&-6&-12\\\end{matrix}\right]
$$
其中：
$$
E_{13}=\left[\begin{matrix}1&0&0\\0&1&0\\-7&0&1\\\end{matrix}\right]
$$
第三次初等行变换为：
$$
\left[\begin{matrix}1&0&0\\0&1&0\\0&-2&1\\\end{matrix}\right]*\left[\begin{matrix}1&2&3\\0&-3&-6\\0&-6&-12\\\end{matrix}\right]=
\left[\begin{matrix}1&2&3\\0&-3&-6\\0&0&0\\\end{matrix}\right]
$$
其中：
$$
E_{23}=\left[\begin{matrix}1&0&0\\0&1&0\\0&-2&1\\\end{matrix}\right]
$$
显然，整个过程可以写作：
$$
E_{23}*E_{13}*E_{12}*\left[\begin{matrix}1&2&3\\4&5&6\\7&8&9\\\end{matrix}\right]=
\left[\begin{matrix}1&2&3\\0&-3&-6\\0&0&0\\\end{matrix}\right]
$$
因为初等矩阵相乘后的矩阵必可逆，因此系数矩阵可以写作：
$$
\left[\begin{matrix}1&2&3\\4&5&6\\7&8&9\\\end{matrix}\right]=(E_{23}*E_{13}*E_{12})^{-1}*
\left[\begin{matrix}1&2&3\\0&-3&-6\\0&0&0\\\end{matrix}\right]
$$