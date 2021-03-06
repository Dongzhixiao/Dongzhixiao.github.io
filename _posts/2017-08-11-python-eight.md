---
layout:     post
title:      "python学习(八)"
subtitle:   "2017/08/11 numpy模块相关函数"
date:       2017-08-11
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年8月11日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

NumPy是高性能科学计算和数据分析的基础包

# numpy的ndarray:一种多维数组对象

## 数组创建函数
|函数|说明|
|-|-|
|array|将输入数据(列表、元祖、数组或其他序列类型)转化为ndarry。要么推断出dtype，要么显示指定dtype。默认直接复制输入数据|
|asarray|将结构数据转化为ndarray，但是和array的主要区别就是当数据源是ndarray时，array仍然会copy出一个副本，占用新的内存，但asarray不会。|
|arange|类似于内置的range，但是返回的是一个ndarray，而不是列表|
|ones、ones_like|根据指定的形状和dtype创建一个全1数组。ones_like以另一个数组为参数，并根据其形状和和dtype创建一个全1数组|
|zeros、zeros_like|类似于ones、ones_like，只不过产生一个全0数组|
|empty、empty_like|创建新数组，只分配内存空间但不填充任何值|
|eye、identity|创建一个正方的N*N单位矩阵(对角线1，其余都为0)|

## ndarray的数据类型
dtype是一个特殊对象，它含有ndarray将一块内存解释为特定数据类型的所有信息，数据类型有:
|类型|类型代码|说明|
|-|-|-|
|int8、uint8|i1、u1|有符号和无符号的8位(1个字节)整型|
|int16、uint16|i2、u2|有符号和无符号的16位(2个字节)整型|
|int32、uint32|i4、u4|有符号和无符号的32位(4个字节)整型|
|int64、uint64|i8、u8|有符号和无符号的64位(8个字节)整型|
|float16|f2|半精度浮点数|
|float32|f4或f|标准单精度浮点数。与C的float兼容|
|float64|f8或d|标准双精度浮点数。与C的double和Python的float对象兼容|
|float128|f16或g|扩展精度浮点数|
|complex64、complex128、complex256|c8、c16、c32|分别用两个32位、64位或128位浮点数表示的复数|
|bool|?|存储True和False值的布尔类型|
|object|O|Python对象类型|
|string_|S|固定长度的字符串类型(每个字符1个字节)。例如，要创建一个长度为10的字符串，应该使用S10|
|unicode_|U|固定长度的unicode类型(字节数由平台决定)。跟字符串的定义方式一样(如U10)|
可以通过ndarray的astype方法显式的转换其dtype：
>注：后面代码使用的是ipython交互环境
```ipython
In [36]: import numpy as np

In [37]: arr = np.array([1,2,3])

In [38]: arr.dtype
Out[38]: dtype('int32')

In [39]: arr.astype(np.float64)
Out[39]: array([ 1.,  2.,  3.])
```
也可使用简洁的类型代码进行转换：
```
In [41]: arr.astype('i1')
Out[41]: array([1, 2, 3], dtype=int8)
```
## 切片和索引

### 切片
数组切片跟列表切片的主要区别是：数组切片是原始数组的视图，即数据不会被复制，视图上任意修改都会直接反应到源数组上：
```
In [42]: arr = np.arange(10)

In [43]: arr
Out[43]: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

In [44]: arr[1:3]
Out[44]: array([1, 2])

In [45]: arr[1:3]=0

In [46]: arr
Out[46]: array([0, 0, 0, 3, 4, 5, 6, 7, 8, 9])
```
>注意：如果想得到ndarray切片的一份副本而非视图，需要显式的进行复制操作，例如：arr[1:3].copy()。

### 索引

#### 元素索引
二维数组的索引不是标量，而是一维数组，但是可以传入两个括号或者一个以逗号隔开的索引列表来选取一个元素。
```
In [47]: arr2 = np.array([[1,2],[3,4],[5,6]])

In [48]: arr2[2]
Out[48]: array([5, 6])

In [50]: arr2[0][1]
Out[50]: 2

In [51]: arr2[0,1]
Out[51]: 2
```

#### 切片索引
ndarray的切片语法和Python差不多，高维度对象可以在一个或者多个轴上进行切片，二维数据的部分切片结果如下图所示：
![图片](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/2DArraySlice.png?raw=true "图片")

#### 布尔型索引
我们用一个数组存名字，一个数组存数据：
```
In [4]: names = np.array(['W','Z','Q','S','L','W','Z','W','Z','W'])
In [6]: data = np.random.randn(10,4)

In [7]: names
Out[7]: 
array(['W', 'Z', 'Q', 'S', 'L', 'W', 'Z', 'W', 'Z', 'W'], 
      dtype='<U1')
In [8]: data
Out[8]: 
array([[ -1.02322105e-01,   5.04230649e-01,   8.04791357e-02,
          2.19664035e+00],
       [ -1.32885684e+00,  -6.51714274e-01,   2.21082584e-01,
         -2.15442212e+00],
       [ -1.67034480e-01,   7.83798621e-01,  -4.32103866e-01,
          3.47590743e-01],
       [ -9.20678801e-01,   2.80369235e+00,  -1.22112155e+00,
          1.32338667e+00],
       [ -8.99608000e-01,  -1.47111491e+00,  -6.91045081e-01,
          4.24236847e-01],
       [  1.00743466e-01,  -1.17750852e+00,   5.92357901e-01,
         -5.65106221e-01],
       [ -1.10900751e+00,  -4.74554754e-01,   8.05406038e-01,
          2.43389782e-01],
       [ -3.77793172e-02,   7.10157528e-01,  -3.98930846e-01,
          1.97301403e+00],
       [  1.35008964e+00,   1.36317897e+00,  -2.65062342e-01,
          8.86795896e-01],
       [ -4.27056196e-01,  -5.86725073e-01,  -4.35025045e-01,
          2.61682607e-03]])
```
假设每个名字对应data数据的一行，我们要选出'W'的所有行，我们可以先得到names和字符串'W'的比较得到一个布尔型数组：
```
In [9]: names == 'W'
Out[9]: array([ True, False, False, False, False,  True, False,  True, False,  True], dtype=bool)
```
这个布尔型数组可以用于数组索引：
```
In [12]: data[names=='W']
Out[12]: 
array([[-0.1023221 ,  0.50423065,  0.08047914,  2.19664035],
       [ 0.10074347, -1.17750852,  0.5923579 , -0.56510622],
       [-0.03777932,  0.71015753, -0.39893085,  1.97301403],
       [-0.4270562 , -0.58672507, -0.43502504,  0.00261683]])
```
布尔型数组的长度必须跟被索引的轴长度一致。还可以将布尔型数组跟切片混合使用：
```
In [13]: data[names=='W',1:3]
Out[13]: 
array([[ 0.50423065,  0.08047914],
       [-1.17750852,  0.5923579 ],
       [ 0.71015753, -0.39893085],
       [-0.58672507, -0.43502504]])
```
#### 花式索引
利用整数数组进行索引
如果我们想要选取特定行的索引，只需传入特定行的的整数的索引数组即可：
```
In [15]: arr = np.empty((5,4))

In [16]: for i in range(5):
    ...:     arr[i]=i
    ...:     

In [17]: arr
Out[17]: 
array([[ 0.,  0.,  0.,  0.],
       [ 1.,  1.,  1.,  1.],
       [ 2.,  2.,  2.,  2.],
       [ 3.,  3.,  3.,  3.],
       [ 4.,  4.,  4.,  4.]])

In [18]: arr[[4,1,3]]
Out[18]: 
array([[ 4.,  4.,  4.,  4.],
       [ 1.,  1.,  1.,  1.],
       [ 3.,  3.,  3.,  3.]])
```
一次传入多个索引数组会得到一个一维数组，其元素是对应各个索引元祖：
```
In [19]: arr = np.arange(20).reshape((5,4))

In [20]: arr
Out[20]: 
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15],
       [16, 17, 18, 19]])
In [23]: arr[[1,3],[0,2]]
Out[23]: array([ 4, 14])
```
如果想要得到行列子集对应的矩形区域，则可以按如下方式操作：
```
In [29]: arr[[1,3]][:,[0,2]]
Out[29]: 
array([[ 4,  6],
       [12, 14]])
```
也可使用np.ix_函数，它可将两个一维数组转换为用于选取方形区域的索引器：
```
In [30]: arr[np.ix_([1,3],[0,2])]
Out[30]: 
array([[ 4,  6],
       [12, 14]])
```
>注意：花式索引和切片不一样，它总是将数据复制到新数组中。
### 数组转置和轴对换
数组转置不仅可用transpose方法，还有一个特殊属性T，在矩阵计算时可以利用np.dot进行矩阵内积的计算：
```
In [32]: arr = np.arange(12).reshape((3,4))

In [33]: arr
Out[33]: 
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])

In [34]: arr.T
Out[34]: 
array([[ 0,  4,  8],
       [ 1,  5,  9],
       [ 2,  6, 10],
       [ 3,  7, 11]])

In [35]: np.dot(arr.T,arr)
Out[35]: 
array([[ 80,  92, 104, 116],
       [ 92, 107, 122, 137],
       [104, 122, 140, 158],
       [116, 137, 158, 179]])
```
对于高维数组，transpose需要得到一个由轴编号组成的元祖才能对这些轴进行转置：
```Python
In [37]: arr = np.arange(16).reshape((2,2,4))

In [38]: arr
Out[38]: 
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7]],

       [[ 8,  9, 10, 11],
        [12, 13, 14, 15]]])
In [40]: arr.T      #简单的转置可以使用.T即可，其实就是进行轴对换
Out[40]: 
array([[[ 0,  8],
        [ 4, 12]],

       [[ 1,  9],
        [ 5, 13]],

       [[ 2, 10],
        [ 6, 14]],

       [[ 3, 11],
        [ 7, 15]]])

In [41]: arr.transpose((1,0,2))
Out[41]: 
array([[[ 0,  1,  2,  3],
        [ 8,  9, 10, 11]],

       [[ 4,  5,  6,  7],
        [12, 13, 14, 15]]])
```

## 通用函数：快速的元素级数组函数
一元ufunc:
|函数|说明|
|-|-|
|abs、fabs|计算整数、浮点数或复数的绝对值。对于非复数值，可以使用更快的fabs|
|sqrt|计算各元素的平方根。相当于arr**0.5|
|square|计算各元素的平方。相当于arr**2|
|exp|计算各元素的指数$$e^x$$|	
|log、log10、log2、log1p|分别为自然对数(底数为e)、底数为10的log、底数为2的log、log(1+x)|
|sign|计算各元素的正负号：1(正数)、0(零)、-1(负数)|
|ceil|计算各元素的ceiling值，即大于等于该值的最小整数|
|floor|计算各元素的floor值，即小于等于该值的最大整数|
|rint|计算各元素值四舍五入到最接近的整数，保留dtype|
|modf|将数组的小数和整数部分以两个独立数组的形式返回|
|isnan|返回一个表示“哪些值是NaN(这不是一个数字)”的布尔型数组|
|isfinite、isinf|分别返回一个表示“哪些元素是有穷的(非inf，非NaN)”或“哪些元素是无穷的”的布尔型数组|
|cos、cosh、sin、sinh、tan、tanh|普通和双曲型三角函数|
|arccos、arccosh、arcsin、arcsinh、arctan、arctanh|反三角函数|
|logical_not|计算各元素notx的真值。相当于-arr|
例如：
```
In [42]: arr = np.arange(10)

In [43]: np.sqrt(arr)
Out[43]: 
array([ 0.        ,  1.        ,  1.41421356,  1.73205081,  2.        ,
        2.23606798,  2.44948974,  2.64575131,  2.82842712,  3.        ])

In [44]: np.exp(arr)
Out[44]: 
array([  1.00000000e+00,   2.71828183e+00,   7.38905610e+00,
         2.00855369e+01,   5.45981500e+01,   1.48413159e+02,
         4.03428793e+02,   1.09663316e+03,   2.98095799e+03,
         8.10308393e+03])
```

二元ufunc:
|函数|说明|
|-|-|
|add|将数组中对应的元素相加|
|subtract|将第一个数组中减去第二个数组中的元素|
|multiply|数组元素相乘|
|divide、floor_divide|除发、向下圆整除(丢弃余数)|
|power|对第一个数组中的元素A，根据第二个数组中的元素B，计算$$A^B$$|
|maximum、fmax|元素级的最大值计算。fmax将忽略NaN|
|minimum、fmin|元素级的最小值计算。fmin将忽略NaN|
|mod|元素级的求模计算(除法的余数)|
|copysign|将第二个数组中的值的符号复制给第一个数组中的值|
|greater、greater_equal、less、less_equal、equal、not_equal|执行元素级的比较运算，最终产生布尔型数组。相当于中缀运算符>、>=、<、<=、==、!=|
|logical_and、logical_or、logical_xor|执行元素级的真值逻辑运算。相当于中缀运算符&、\|、^|


例如：
```
In [46]: x = np.random.randn(5)

In [47]: y = np.random.randn(5)

In [48]: x
Out[48]: array([-0.80078384, -1.45147126,  0.43734403, -0.40179986,  0.21811681])

In [49]: y
Out[49]: array([-0.0983136 , -0.94142068,  0.45797322,  0.01221323, -0.97017549])

In [50]: np.maximum(x,y)
Out[50]: array([-0.0983136 , -0.94142068,  0.45797322,  0.01221323,  0.21811681])
```
## 利用数组进行数据处理
假设我们要在一组值（网络型）上计算函数$$sqrt(x^2+y^2)$$。np.meshgrid函数接收两个一维数组，产生两个二维矩阵对应两个数组中所有(x,y)对。
```
In [53]: points = np.arange(-5,5,0.01)

In [54]: xs,ys = np.meshgrid(points,points)

In [55]: xs
Out[55]: 
array([[-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       ..., 
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99],
       [-5.  , -4.99, -4.98, ...,  4.97,  4.98,  4.99]])

In [56]: ys
Out[56]: 
array([[-5.  , -5.  , -5.  , ..., -5.  , -5.  , -5.  ],
       [-4.99, -4.99, -4.99, ..., -4.99, -4.99, -4.99],
       [-4.98, -4.98, -4.98, ..., -4.98, -4.98, -4.98],
       ..., 
       [ 4.97,  4.97,  4.97, ...,  4.97,  4.97,  4.97],
       [ 4.98,  4.98,  4.98, ...,  4.98,  4.98,  4.98],
       [ 4.99,  4.99,  4.99, ...,  4.99,  4.99,  4.99]])
In [57]: import matplotlib.pyplot as plt

In [58]: z = np.sqrt(xs**2+ys**2)

In [59]: z
Out[59]: 
array([[ 7.07106781,  7.06400028,  7.05693985, ...,  7.04988652,
         7.05693985,  7.06400028],
       [ 7.06400028,  7.05692568,  7.04985815, ...,  7.04279774,
         7.04985815,  7.05692568],
       [ 7.05693985,  7.04985815,  7.04278354, ...,  7.03571603,
         7.04278354,  7.04985815],
       ..., 
       [ 7.04988652,  7.04279774,  7.03571603, ...,  7.0286414 ,
         7.03571603,  7.04279774],
       [ 7.05693985,  7.04985815,  7.04278354, ...,  7.03571603,
         7.04278354,  7.04985815],
       [ 7.06400028,  7.05692568,  7.04985815, ...,  7.04279774,
         7.04985815,  7.05692568]])

In [62]: plt.imshow(z,cmap=plt.cm.gray);plt.colorbar();plt.title("Image plot of $\sqrt{x^2+y^2}$ for a grid of values")
Out[62]: <matplotlib.text.Text at 0xddbef98>
```
会出现图片：
![图片](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/numpy_1.png?raw=true "图片")
>这张图是使用matplotlib.pyplot.imshow函数创建的

### 条件逻辑的数组运算
np.where是x if condition else y 的矢量化版本。
假设一个随机矩阵，将所有正数替换为1，负数替换为-1，可以这样使用：
```
In [63]: arr = np.random.randn(3,3)

In [64]: arr
Out[64]: 
array([[-0.4048126 , -0.7349476 , -0.55646179],
       [ 0.33211433, -0.78405077, -0.6061113 ],
       [ 0.07675773, -2.03656668, -0.08453712]])

In [65]: np.where(arr>0,1,-1)
Out[65]: 
array([[-1, -1, -1],
       [ 1, -1, -1],
       [ 1, -1, -1]])
```

### 数学和统计方法
|方法|说明|
|-|-|
|sum|对数组中全部或某轴向的元素求和。零长度的数组的sum为0|
|mean|算术平均数。零长度的数组的mean为NaN|
|std、var|分别为标准差和方差，自由度可调(默认为n)|
|min、max|最小值、最大值|
|argmin、argmax|分别为最小和最大元素的索引|
|cumsum|所有元素的累计和|
|cumprod|所有元素的累计积|
例如：
```
In [67]: arr = np.array(([1,2],[3,4]))

In [68]: arr
Out[68]: 
array([[1, 2],
       [3, 4]])

In [69]: arr.mean()
Out[69]: 2.5

In [70]: np.mean(arr)
Out[70]: 2.5

In [71]: arr.sum()
Out[71]: 10
```
mean和sum函数可接受axis参数，计算该轴上的统计值，结果是少一维的数组：
```
In [75]: arr.sum(1)
Out[75]: array([3, 7])

In [76]: arr.sum(axis = 0)
Out[76]: array([4, 6])
```
### 排序
使用sort函数排序，一维数组直接排序，多维数组可以传入参数表示任意一个轴上进行排序：
```
In [83]: arr = np.random.randn(2,3)

In [84]: arr
Out[84]: 
array([[ 0.19225798,  0.41109903, -0.13768052],
       [ 0.86202103, -0.11825986,  1.30009857]])

In [85]: arr.sort(0)  #每一列进行排序

In [86]: arr
Out[86]: 
array([[ 0.19225798, -0.11825986, -0.13768052],
       [ 0.86202103,  0.41109903,  1.30009857]])
       
In [90]: arr.sort(1)  #每一行进行排序

In [91]: arr
Out[91]: 
array([[-0.13768052, -0.11825986,  0.19225798],
       [ 0.41109903,  0.86202103,  1.30009857]])
```

顶级方法np.sort返回的是副本，直接自己后面带的sort方法会修改自身。
>计算分位数的最简单办法就是对其排序后选取特定位置的值：
```
In [92]: arr = np.random.randn(1000)

In [93]: arr.sort()

In [94]: arr[int(0.05*len(arr))] #5%分位数
Out[94]: -1.6654452453540165
```

### 集合逻辑
|方法|说明|
|-|-|
|unique(x)|计算x中的唯一元素，并返回有序结果|
|intersect1d(x,y)|计算x和y中的公共元素，并返回有序结果|
|union1d(x,y)|计算x，y的并集，并返回有序结果|
|in1d(x,y)|得到一个表示“x的元素是否包含于y”的布尔型数组|
|setdiff1d(x,y)|集合的差，即元素在x中且不在y中|
|setxor1d(x,y)|集合的对称差，即存在于一个数组中但不同时存在于两个数组中的元素|

例如：
```
In [95]: arr = np.array([1,2,2,3,3,3,4,4,4,4,5,5,5,5,5]);

In [96]: arr
Out[96]: array([1, 2, 2, 3, 3, 3, 4, 4, 4, 4, 5, 5, 5, 5, 5])

In [97]: np.unique(arr)
Out[97]: array([1, 2, 3, 4, 5])

In [98]: np.in1d(arr,[2,3,4])
Out[98]: 
array([False,  True,  True,  True,  True,  True,  True,  True,  True,
        True, False, False, False, False, False], dtype=bool)
```

## 数组文件输入输出
### 存取二进制文件
np.save和np.load是读写数组数据的主要函数。默认数组以未压缩原始二进制格式保存在扩展名为.npy的文件中。
```
In [99]: import os

In [100]: os.getcwd()
Out[100]: 'C:\\Users\\Administrator'

In [101]: arr = np.arange(10)

In [102]: np.save('arr',arr)
```
以上代码会在`C:\\Users\\Administrator`路径下生成`arr.npy`的文件，这里保存时文件没有加后缀，默认会自动加上。此时可以使用np.load()函数读取：
```
In [103]: np.load('arr.npy')
Out[103]: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```
np.savez可保存多个数组，数组以关键字参数传入，读取时会得到一个类似字典的对象，会对各个数组进行延迟加载：
```
In [104]: np.savez('arrs',a=arr,b=arr)

In [105]: l = np.load('arrs.npz')

In [106]: l['a']
Out[106]: array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

```
>注意此时的默认后缀是`npz`

### 存取文本文件
读写文本文件可以使用np.savetxt和np.loadtxt函数进行，如下所示：
```
In [116]: arr = np.arange(16).reshape(4,4)

In [117]: arr
Out[117]: 
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11],
       [12, 13, 14, 15]])

In [118]: np.savetxt('savetxt.txt',arr,delimiter=',')

In [119]: np.loadtxt('savetxt.txt',delimiter=',')
Out[119]: 
array([[  0.,   1.,   2.,   3.],
       [  4.,   5.,   6.,   7.],
       [  8.,   9.,  10.,  11.],
       [ 12.,  13.,  14.,  15.]])

```

## 线性代数
常用的numpy.linalg下面的函数
|函数|说明|
|-|-|
|diag|以一维数组的形式返回方阵的对角线(或非对角线)元素，或将一维数组转换为方阵(非对角元素为0)|
|dot|矩阵乘法：对于二维矩阵，计算真正意义上的矩阵乘积，同线性代数中矩阵乘法的定义(相当于matlab的“\*”))。对于一维矩阵，计算两者的内积。注意——矩阵对于元素相乘直接使用乘号“*”(相当于matlab的“.\*”)|
|trace|计算对角线元素的和|
|det|计算矩阵行列式|
|eig|计算方阵的本征值和本征向量|
|inv|计算方阵的逆|
|pinv|计算矩阵的Moore-Penrose伪逆|
|qr|计算QR分解|
|svd|计算奇异值分解(SVD)|
|solve|解线性方程组Ax=b,其中A为一个方阵|
|lstsq|计算Ax=b的最小二乘解|
例如：
```
In [133]: arr1 = np.array([[1],[2]])

In [134]: arr1
Out[134]: 
array([[1],
       [2]])

In [135]: arr2 = np.array([[3,4]])

In [136]: arr2
Out[136]: array([[3, 4]])

In [137]: arr1*arr2 #等价于arr1.dot(arr2)等价于np.dot(arr1,arr2)
Out[137]: 
array([[3, 4],
       [6, 8]])

In [138]: X = np.random.randn(2,2)

In [139]: X
Out[139]: 
array([[ 0.88292605,  1.01818481],
       [ 0.30069419, -1.08288059]])

In [140]: mat = X.T.dot(X)

In [141]: mat
Out[141]: 
array([[ 0.8699754 ,  0.57336598],
       [ 0.57336598,  2.20933069]])

In [26]: arr1 = np.array([[1,0],[0,1]])
    ...: 
    ...: arr2 = np.array([[1,2],[3,4]])
    ...: 

In [27]: arr1
Out[27]: 
array([[1, 0],
       [0, 1]])

In [28]: arr2
Out[28]: 
array([[1, 2],
       [3, 4]])

In [29]: arr1.dot(arr2)
Out[29]: 
array([[1, 2],
       [3, 4]])

In [30]: arr1*arr2
Out[30]: 
array([[1, 0],
       [0, 4]])

In [142]: np.linalg.inv(mat)
Out[142]: 
array([[ 1.38662535, -0.35985731],
       [-0.35985731,  0.54601602]])

In [143]: mat.dot(np.linalg.inv(mat))
Out[143]: 
array([[  1.00000000e+00,   0.00000000e+00],
       [  1.11022302e-16,   1.00000000e+00]])

In [145]: q,r = np.linalg.qr(mat)

In [146]: r
Out[146]: 
array([[-1.04192406, -1.69452788],
       [ 0.        ,  1.52920435]])
```

## 随机数生成
常用的numpy.random下面的函数
|函数|说明|
|-|-|
|seed|确定随机数生成器的种子|
|permutation|返回一个序列的随机排列或返回一个随机排列的范围|
|shuffle|对一个序列就地随机排列|
|rand|产生均匀分布的样本值|
|randint|从给定的上下限范围内随机选取整数|
|randn|产生正态分布(平均值为0，标准差为1)的样本值，类似于Matlab接口|
|binomial|产生二项分布的样本值|
|normal|产生正态(高斯)分布的样本值|
|beta|产生Beta分布的样本值|
|chisquare|产生卡方卡方分布的样本值|
|gamma|产生Gamma分布的样本值|
|uniform|产生在[0,1)中均匀分布的样本值|
例如：
```
In [149]: samples = np.random.chisquare(3,size = (4,4)) #产生4*4的自由度为3的卡方分布的矩阵

In [150]: samples
Out[150]: 
array([[ 1.27379583,  7.45545193,  4.9337084 ,  1.5294834 ],
       [ 2.9062776 ,  0.37658176,  0.95044702,  2.96017035],
       [ 1.39309485,  0.12209938,  1.07898882,  1.16941599],
       [ 1.02115297,  1.14074292,  0.29333573,  2.37203726]])
```