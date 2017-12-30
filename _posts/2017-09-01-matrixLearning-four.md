---
layout:     post
title:      "矩阵基础(四)"
subtitle:   "向量的线性相关性  向量张成的空间  向量空间的基 正交向量和正交空间 求解无解方程的最优解"
date:       2017-09-01
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170817.jpg?raw=true"
tags:
    - 矩阵学习
---


### 时间:2017年9月1日 天气:阴:cloud:
-----
#####   Author:冬之晓
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

##向量的线性相关性
如果有一组向量：
$$
x_1,x_2,...,x_n
$$
对于线性组合：
$$
c_1x_1+c_2x_2+c_3x_3+...+c_nx_n=0
$$
只有当：
$$
C_i(i=1,2,...,n)
$$
全部为零时才成立。
那么说明
$$
x_1,x_2,...,x_n
$$
线性无关。
如果把这组向量组合成m行n列的矩阵：
$$
A=[x_1,x_2,...,x_n]
$$
不相关即等价于方程：
$$
Ax=0
$$
只有一个0解，也即矩阵A的零空间N(A)只有零向量，矩阵A的秩R(A)=n，矩阵A没有自由变量；
如果线性相关则说明有非零解，也即矩阵A的零空间N(A)有非零向量，矩阵A的秩R(A) < n，矩阵A含有自由变量。

##向量张成的空间
如果有向量：
$$
x_1,x_2,...,x_n
$$
我们称这些向量的所有线性组合就是这些向量张成的空间。
##向量空间的基
如果有向量：
$$
x_1,x_2,...,x_n
$$
他们满足：
1.线性无关
2.张成一个m(m可以小于向量中元素的个数)维空间
则称这些向量是该m维空间的一组基向量。
如果得到一个空间的一组基，就相当于有了这个空间的所有信息啦！

- 显然，对于m行n列的矩阵A来说，矩阵A的秩R(A)就是**矩阵A的列空间C(A)的维数**。
- **矩阵A的零空间N(A)的维数**等于n-R(A)，这个值即是我们使用前面讲的消元法得到的自由列的数目。
- 因为$$R(A)=R(A^T)$$所以**矩阵A的行空间$$C(A^T)$$的维数**等于秩R(A)。
- 显然，**矩阵A转置的零空间（左零空间）的维数**等于m-R(A)。

------------------

下面，然我们看看如何求矩阵中这四个空间的基。
我们可以通过行变换最简矩阵的方式：
$$
rref[A_{m*n}|I_{n*n}]\Longrightarrow[R_{m*n}|E_{n*n}]
$$

- 显然，R矩阵中主元对应的行就是**矩阵A的行空间$$C(A^T)$$的基**。（行变换不改变矩阵的行空间，即C(A)=C(R)，R的每一行都是A原来行的线性组合）
- R矩阵中主元对应的列数在A中取得的那些列就是**矩阵A的列空间C(A)的基**。
- 使用前面讲的消元法得到的方程组的自由变量就是**矩阵A的零空间N(A)的基**。
- 因为$$E_{n*n}A_{m*n}=R_{m*n}$$所以找到矩阵R中零向量所对应的行数在E中取得的那些行就是**矩阵A转置的零空间（左零空间）的基**

##正交向量和正交空间
如果向量x,y满足：
$$
x^Ty=0
$$
则说明x和y向量正交。
如果两个空间中分别任意取一个向量，然后都正交，则说明这两个空间正交。
对应矩阵的四个主要空间来说：
矩阵A的行空间和矩阵A的零空间相互正交。
因为
$$
Ax=0
$$
可以看出，零空间x的解和矩阵A的每一行向量乘积都是0。
同理，矩阵A的列空间和矩阵A的左零空间相互正交。

##求解无解方程的最优解
假设方程：
$$
Ax=b
$$
无解，我们将两边同时左乘A的转置，得到方程：
$$
A^TA\hat{x}=A^Tb
$$
这个方程有解。这个解就是方程的最优解。
关于原因，我们先从一维的情况看：
![误差图](http://img.blog.csdn.net/20170902000250692?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMTk1Mjg5NTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
有b向量在向量a上面的最优解是垂直于a向量的投影的p向量。
为了求得这个最优解，我们可以看p属于a的子空间，因此可以设p=ax。而误差e=b-p。由于a和e垂直，我们可以得到：
$$
a^Te=0\Longrightarrow\\
a^T(b-p)=0\Longrightarrow\\
a^T(b-ax)=0\Longrightarrow\\
a^Tax=a^Tb\Longrightarrow\\
x=(a^Ta)^{-1}a^Tb=\frac{a^Tb}{a^Ta}
$$
之后也可以计算投影
$$
p=ax=\frac{aa^T}{a^Ta}b\\
正交投影矩阵就是：P=\frac{aa^T}{a^Ta}\\
满足性质：\\
1.P^T=P\\
2.R(P)=1\\
3.P^n=P^{n-1}=...=P^2=P
$$
显然，对应方程
$$
Ax=b
$$
无解的时候，我们需要微调b使得其落在A的空间中，这就需要用A的正交投影矩阵乘以b，使得微调后的方程
$$
A\hat{x}=p
$$
有解。
若想求解该问题，先看下图：
![这里写图片描述](http://img.blog.csdn.net/20170902115331421?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvcXFfMTk1Mjg5NTM=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
假设图上的平面是高维空间上的超平面，也就是矩阵A列空间所确定的超平面空间，由一组基：
$$
a_1,a_2,...a_n
$$
构成。
显然，如果向量b不在该超平面上，则方程
$$
Ax=b
$$
肯定无解，因此需要将b正交投影到该平面上，得到p，这样才能得到近似解：
$$
A\hat{x}=p
$$
为了找到该方程的解，我们从几何的角度考虑，因为误差e向量与C(A)超平面正交，因此：
$$
A^Te=0\cdots\cdots(1)
$$
同时：
$$
e=b-p=b-A\hat{x}\cdots\cdots(2)
$$
显然，e在A的转置的零空间(A的左零空间)中，我们将(2)带入(1)式中，得到：
$$
A^T(b-A\hat{x})=0\Longrightarrow\\
A^TA\hat{x}=A^Tb\Longrightarrow\\
\hat{x}=(A^TA)^{-1}A^Tb
$$
这时已经得到近似解，但是我们还可以更进一步，得到A的正交投影矩阵P，因为：
$$
p=A\hat{x}=A(A^TA)^{-1}A^Tb且p=Pb
$$
对比上面两个式子可得A的正交投影矩阵：
$$
P=A(A^TA)^{-1}A^T
$$
>思考一下，当A可逆时，A的正交投影矩阵可以化简：$$P=AA^{-1}(A^T)^{-1}A^T=I$$即，单位阵，为什么？因为当A可逆时，A的列空间可以张成到整个空间，因此空间中的任意向量投影在整个空间中后还是原向量，因此此时A的正交投影矩阵只能是单位阵。