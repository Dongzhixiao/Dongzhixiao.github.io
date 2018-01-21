---
layout:     post
title:      "矩阵基础(二)"
subtitle:   "2017/08/24 高斯约当法消元法求矩阵的逆  LU分解与行置换矩阵  用高斯消元法解线性方程组"
date:       2017-08-24
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170817.jpg?raw=true"
tags:
    - 矩阵学习
---


### 时间:2017年8月24日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

## 高斯约当法消元法求矩阵的逆
矩阵的可逆性，矩阵A如果存在非零向量x使得：
$$Ax=0$$这说明矩阵A不可逆。
如果矩阵A可逆，则：

$$AA^{-1}=A^{-1}A=I(单位矩阵)$$

>这里先仅仅考虑方阵，非方阵则需要考虑左右逆，因此需要去掉中间的等式。

假设可逆矩阵：

$$
A=\left[\begin{matrix}1&2\\4&5\\\end{matrix}\right]
$$

假设其逆矩阵：

$$
A^{-1}=\left[\begin{matrix}a&b\\c&d\\\end{matrix}\right]
$$

则满足：

$$
A*A^{-1}=\left[\begin{matrix}1&2\\4&5\\\end{matrix}\right]*
\left[\begin{matrix}a&b\\c&d\\\end{matrix}\right]=\left[\begin{matrix}1&0\\0&1\\\end{matrix}\right]
$$

求解逆矩阵可以看做求解两个线性方程组：

$$
\left[\begin{matrix}1&2\\4&5\\\end{matrix}\right]*
\left[\begin{matrix}a\\c\\\end{matrix}\right]=\left[\begin{matrix}1\\0\\\end{matrix}\right]\\
\left[\begin{matrix}1&2\\4&5\\\end{matrix}\right]*
\left[\begin{matrix}b\\d\\\end{matrix}\right]=\left[\begin{matrix}0\\1\\\end{matrix}\right]
$$

按照矩阵基础（一）中的“用高斯消元法解线性方程组”我们可以使用左乘初等矩阵进行行变化分别解得未知数a、b、c、d。但是这时约当跟高斯说，一起算吧，于是就把他们两个放在一起，变成一个曾广矩阵来进行行变换，即：

$$
[A|I]\Longrightarrow\left[\begin{matrix}1&2&1&0\\4&5&0&1\\\end{matrix}\right]\Longrightarrow[I|E]\Longrightarrow\left[\begin{matrix}1&0&-\frac{5}{3}&\frac{2}{3}\\0&1&\frac{4}{3}&\frac{1}{3}\\\end{matrix}\right]
$$

左乘初若干个初等矩阵的乘积E使得矩阵A变为单位阵，这样显然该初等矩阵E就是所要求得A的逆矩阵，即：

$$
A^{-1}=E=\left[\begin{matrix}-\frac{5}{3}&\frac{2}{3}\\\frac{4}{3}&\frac{1}{3}\\\end{matrix}\right]
$$

## LU分解与行置换矩阵
对于n阶矩阵，在不需要进行置换行的时候，进行LU分解需要的次数是：

$$
n^2+(n-1)^2+\cdots+2^2+1^2\thickapprox\frac{1}{3}n^3
$$

不过大部分情况下矩阵能够进行LU分解时都需要进行行互换，对于n阶矩阵，其行置换矩阵有n的阶乘个。
比如，3阶方阵的行置换矩阵有一下3*2种：

$$
\left[\begin{matrix}1&0&0\\0&1&0\\0&0&1\\\end{matrix}\right]
\left[\begin{matrix}0&1&0\\1&0&0\\0&0&1\\\end{matrix}\right]
\left[\begin{matrix}0&0&1\\0&1&0\\1&0&0\\\end{matrix}\right]\\
\left[\begin{matrix}1&0&0\\0&0&1\\0&1&0\\\end{matrix}\right]
\left[\begin{matrix}0&1&0\\0&0&1\\1&0&0\\\end{matrix}\right]
\left[\begin{matrix}0&0&1\\1&0&0\\0&1&0\\\end{matrix}\right]
$$

设行置换矩阵为P，则：

$$
P^{-1}=P^T
$$

一般情况下矩阵的LU分解为以下形式：

$$
PA=LU
$$

## 对称矩阵
设矩阵A满足：

$$
A^T=A
$$

则A就是对称矩阵，一般对称矩阵可有由任意矩阵R，通过

$$
R^TR
$$

得到。
因为：

$$
(R^TR)^T=R^T(R^T)^T=R^TR
$$
