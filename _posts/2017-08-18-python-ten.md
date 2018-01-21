---
layout:     post
title:      "python学习(十)"
subtitle:   "2017/08/18 matplotlib模块相关函数"
date:       2017-08-18
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年8月18日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

matplotlib模块是用来创建图表的桌面绘图包。
在ipython中使用matplotlib画图要先使用%matbplolib选择交互的方式：
```
In [1]: from PyQt5 import QtCore, QtGui #导入绘图包
In [2]: %matplotlib -l #查看能够选择的交互方式
Available matplotlib backends: ['tk', 'gtk', 'gtk3', 'wx', 'qt', 'qt4', 'qt5', 'osx', 'nbagg', 'notebook', 'agg', 'inline', 'ipympl']
Available matplotlib backends: ['tk', 'gtk', 'gtk3', 'wx', 'qt', 'qt4', 'qt5', 'osx', 'nbagg', 'notebook', 'agg', 'inline', 'ipympl']

In [3]: %matplotlib qt5  #前面导入的是Qt5，因此这里使用qt5作为交互环境

In [4]: import matplotlib.pyplot as plt
In [5]: plt.plot([1,2])
Out[6]: [<matplotlib.lines.Line2D at 0xbeff470>]
```
此时会出现一个新窗口，上面画着一条直线。后面就可以进行下一步学习了。
## Figure和subplot
### 绘图
matplotlib的图像都在Figure对象中。可以使用plt.fiture创建新的Figure。
```
In [13]: fig = plt.figure()
```
会弹出新的空白窗口。其选项figsize确定保存时图片的纵横比。这个函数还支持类似matlab的编号功能——plt.figure(2)。plt.gcf()可得到当前Figure的引用。
有了Figure对象后，可以使用add_subplot函数增加图像数目：
```
In [6]: fig1 = fig.add_subplot(2,2,1)

In [7]: fig2 = fig.add_subplot(2,2,2)

In [8]: fig3 = fig.add_subplot(2,2,3)

In [9]: import numpy as np

In [10]: plt.plot(np.random.randn(50).cumsum(),'k--')
Out[10]: [<matplotlib.lines.Line2D at 0xbe35a20>]
```
这样就会使得图像排布为2*2，然后最后画的图像出现在第三个图像上面。如果想要在第一个和第二个图像上面画图，可以使用创建时的返还对象的引用fig1和fig2：
```
In [11]: fig1.plot(np.random.randn(50).cumsum(),'k--')
```
上一行代码就是在第一个图像上绘图。
### 保存
绘制完图像后，可以使用Figure.savefig函数进行图像的保存。其选项有：
|参数|说明|
|-|-|
|fname|含有文件路径的字符串或Python的文件型对象。图像格式由文件扩展名推断得出。|
|dpi|图像分辨率(每英寸的点数)，默认为100|
|facecolor、edgecolor|图像的背景色，默认为'w'(白色)|
|format|显示设置文件格式("png"、"pdf"、"svg"、"ps"、"eps"……)|
|bbox_inches|图表需要保存的部分。如果设置为"tight"，则将尝试剪除图表周围的空白部分|
## pandas中的绘图函数
更多时候，我们喜欢使用panda处理数据并直接绘图，这是就可以直接使用pandas数据结构本身的绘图函数。pandas里面的两种数据结构Series和DataFrame都可以进行绘图：

- Series.plot方法的参数和说明表格：
|参数|说明|
|-|-|
|label|用于图例的标签|
|ax|要在其上进行绘制的matplotlib subplot对象。如果没有设置，则使用当前matplotlib subplot|
|style|将要传给matplotlib的风格字符串(如'ko--')|
|alpha|图表的填充不透明度(0到1之间)|
|kind|可以是'line'、'bar'、'barh'、'kde'|
|logy|在y轴上使用对数标尺|
|use_index|将对象的索引用作刻度标签|
|rot|旋转刻度标签(0到360)|
|xticks|用作x轴刻度的值|
|yticks|用作y轴刻度的值|
|xlim|x轴的界限(例如[0,10])|
|ylim|y轴的界限|
|grid|显示轴网格线(默认打开)|

- Series.plot方法的参数和说明表格：
|参数|说明|
|-|-|
|subplots|将各个DataFrame列绘制到单独的subplot中|
|sharex|如果subplots=True，则共用同一个X轴，包括刻度和界限|
|sharey|如果subplots=True，则共用同一个Y轴|
|figsize|表示图像大小的元组|
|title|表示图像标题的字符串|
|legend|添加一个subplot图例(默认为True)|
|sort_columns|以字母顺序绘制各列，默认使用当前列顺序|