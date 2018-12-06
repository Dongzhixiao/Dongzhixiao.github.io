
# 目录

<!-- @import "[TOC]" {cmd="toc" depthFrom=1 depthTo=6 orderedList=false} -->

<!-- code_chunk_output -->

* [目录](#目录)
* [使用Python语言进行机器学习工作流的实例分析](#使用python语言进行机器学习工作流的实例分析)
	* [1. 介绍](#1-介绍目录)
	* [2. 机器学习工作流程](#2-机器学习工作流程目录)
	* [3 问题定义](#3-问题定义目录)
		* [3.1 问题特征](#31-问题特征)
		* [3.2 目标](#32-目标)
		* [3.3 变量](#33-变量)
	* [4. 输入输出](#4-输入输出目录)
	* [5. 安装工具和依赖包](#5-安装工具和依赖包目录)
	* [6. 探索性数据分析](#6-探索性数据分析目录)
		* [6.1 数据搜集与简单探索](#61-数据搜集与简单探索)
		* [6.2 可视化](#62-可视化)
			* [6.2.1 散点图](#621-散点图)
			* [6.2.2 盒图](#622-盒图)
			* [6.2.2 盒图(附)](#622-盒图附)
			* [6.2.3 条形图](#623-条形图)
			* [6.2.4 双变量的散点图](#624-双变量的散点图)
			* [6.2.5 小提琴图](#625-小提琴图)
			* [6.2.6 成对图](#626-成对图)
			* [6.2.7 kdeplot](#627-kdeplot)
			* [6.2.8 jointplot](#628-jointplot)
			* [6.2.9 安德鲁斯曲线](#629-安德鲁斯曲线)
			* [6.2.10 热图](#6210-热图)
			* [6.2.11 雷达图](#6211-雷达图)
		* [6.3 数据预处理](#63-数据预处理)
		* [6.4 数据清洗](#64-数据清洗)
	* [7. 模型探索](#7-模型探索目录)
		* [7.1 K近邻算法](#71-k近邻算法)
		* [7.2 限定半径最近邻算法](#72-限定半径最近邻算法)
		* [7.3 逻辑回归(线性模型)](#73-逻辑回归线性模型)
		* [7.4 被动攻击分类 (线性模型)](#74-被动攻击分类-线性模型)
		* [7.5 朴素贝叶斯](#75-朴素贝叶斯)
		* [7.6 多项式朴素贝叶斯分类器](#76-多项式朴素贝叶斯分类器)
		* [7.7 伯努利朴素贝叶斯分类器](#77-伯努利朴素贝叶斯分类器)
		* [朴素贝叶斯类算法小结](#朴素贝叶斯类算法小结)
		* [7.8 支持向量机](#78-支持向量机)
		* [7.9 Nu-Support Vector Classification](#79-nu-support-vector-classification)
		* [7.10 线性支持向量机](#710-线性支持向量机)
		* [支持向量机小结](#支持向量机小结)
		* [7.11 决策树](#711-决策树)
		* [7.12 额外决策树](#712-额外决策树)
		* [决策树小节](#决策树小节)
		* [7.13 神经网络](#713-神经网络)
		* [集成学习概述](#集成学习概述)
		* [7.14 随机森林](#714-随机森林)
		* [7.15 Bagging](#715-bagging)
		* [7.16 自适应提示分类器Adaboost](#716-自适应提示分类器adaboost)
		* [7.17 梯度提升分类器GradientBoosting](#717-梯度提升分类器gradientboosting)
		* [7.18 线性判别分析LDA](#718-线性判别分析lda)
		* [7.19 二次判别分析QDA](#719-二次判别分析qda)
	* [8. 总结](#8-总结目录)
	* [9. 参考文献](#9-参考文献目录)

<!-- /code_chunk_output -->


# 使用Python语言进行机器学习工作流的实例分析
姓名：冬之晓
时间：2018年11月2日，北京

## 1. [介绍](#目录)
- IRIS(/’aɪrɪs/鸢yuān尾属植物)的全面ML技术
    - 关于IRIS数据集[[1]](#1)的机器学习技术
    - 帮助指导机器学习的问题的处理过程
    - 用python包作为一个全面的工作流来解决一个简单的机器学习问题
    - 学习处理一般数据科学问题的工作流

## 2. [机器学习工作流程](#目录)
- 机器学习的工作流程
    - 定义问题
    - 指定输入和输出
    - 探索性数据分析
    - 数据采集
    - 数据预处理
    - 数据清理
    - 可视化
    - 模型设计
    - 模型部署
    - 模型维护，诊断

## 3 [问题定义](#目录)
- 当开始一个新的机器学习项目时，重要的事情之一是定义问题
- 问题定义有四个步骤，如下图所示：

![](https://github.com/Dongzhixiao/PictureCache/blob/master/machineLearning/20181102_1.png?raw=true)


### 3.1 问题特征
- 我们将使用经典的Iris数据集。该数据集包含有关三种不同类型鸢尾花的信息：
    - 变色鸢尾
    - Iris Virginica
    - Iris Setosa

- 数据集包含四个变量的度量：
    - 萼片长度
    - 萼片宽度
    - 花瓣长度
    - 花瓣宽度

- Iris数据集有许多有趣的特点：
    - 其中一个类（Iris Setosa）与其他两个类是线性可分的。但是，其他两个类不是线性可分的
    - Versicolor和Virginica类之间存在一些重叠，因此不太可能实现完美的分类
    - 四个输入变量中存在一些冗余，因此可以只用其中三个来实现一个好的解决方案，或者甚至（有困难）用两个，但是最佳变量的精确选择并不明显

### 3.2 目标
- 目标是从萼片和花瓣的长度和宽度的测量中将虹膜花分为三个物种（setosa，versicolor或virginica）

### 3.3 变量
- 变量是：
    - sepal_length：萼片长度，以厘米为单位，用作输入 
    - sepal_width：用作输入的萼片宽度，以厘米为单位 
    - petal_length：用作输入的花瓣长度，以厘米为单位 
    - petal_width：用作输入的花瓣宽度，以厘米为单位 
    - setosa：Iris setosa，真或假，用作目标
    - Iris versicolour，真或假，用作目标 
    - virginica：Iris virginica，真或假，用作目标

## 4. [输入输出](#目录)
- 鸢尾花数据集简单介绍
    - Iris是机器学习中非常流行的分类问题，就像“Hello world”程序
    - 鸢尾花数据集是多变量数据集，由英国统计学家和生物学家罗纳德·费希尔（Ronald Fisher）在其1936年的论文中介绍了在分类学问题中使用多种测量方法作为线性判别分析的一个例子
    - 它有时被称为安德森的数据集，因为埃德加安德森收集了数据来量化三个相关物种中鸢尾花的形态变异。这三个物种中的两个是在加斯佩半岛收集的，“所有这些都来自同一个牧场，并在同一天采摘并由同一个人用同一个设备同时测量”
    - 该数据集由来自三种鸢尾（Iris setosa，Iris virginica和Iris versicolor）中的每一种的50个样品组成
    - 从每个样品测量四个特征：萼片和花瓣的长度和宽度，以厘米为单位

![](https://github.com/Dongzhixiao/PictureCache/blob/master/machineLearning/20181102_2.png?raw=true)

## 5. [安装工具和依赖包](#目录)
- Windows：
    - [Anaconda](https://www.continuum.io)是SciPy的免费Python发行版。它也适用于Linux和Mac
    - [Canopy](https://www.enthought.com/products/canopy)免费提供商业发行版，并提供适用于Windows，Linux和Mac的完整SciPy
    - Python（x，y）是一个免费的Python发行版，包含SciPy和适用于Windows操作系统的Spyder IDE。（可从[这里](http://python-xy.github.io/)下载）

![](https://github.com/Dongzhixiao/PictureCache/blob/master/machineLearning/20181102_3.png?raw=true)

```python {cmd="G:\\Anaconda3\\python.exe" output='html'}
# packages to load 
# Check the versions of libraries
# Python version
import warnings
warnings.filterwarnings('ignore')
import sys
print('Python: {}'.format(sys.version))
# scipy
import scipy
print('scipy: {}'.format(scipy.__version__))
import numpy
# matplotlib
import matplotlib
print('matplotlib: {}'.format(matplotlib.__version__))
# numpy
import numpy as np # linear algebra
print('numpy: {}'.format(np.__version__))
# pandas
import pandas as pd # data processing, CSV file I/O (e.g. pd.read_csv)
print('pandas: {}'.format(pd.__version__))
import seaborn as sns
print('seaborn: {}'.format(sns.__version__))
sns.set(color_codes=True)
import matplotlib.pyplot as plt
print('matplotlib: {}'.format(matplotlib.__version__))
#matplotlib inline
# scikit-learn
import sklearn
print('sklearn: {}'.format(sklearn.__version__))
# Input data files are available in the "../input/" directory.
# For example, running this (by clicking run or pressing Shift+Enter) will list the files in the input directory
import os
#matplotlib inline
from sklearn.metrics import accuracy_score
# Importing metrics for evaluation
from sklearn.metrics import confusion_matrix
from sklearn.metrics import classification_report
```

## 6. [探索性数据分析](#目录)

- 在本节中，学习如何使用图形和数值技术来探索数据的结构
    - 哪些变量暗示了有趣的关系
    - 哪些变量是不寻常的
- 分析和统计操作的基本步骤
    - 6.1 数据收集
    - 6.2 可视化
    - 6.3 数据预处理
    - 6.4 数据清理

### 6.1 数据搜集与简单探索

- 数据收集是收集和测量数据、信息或任何感兴趣的变量的过程，以标准化和确定的方式，使收集器能够回答或测试假设并评估特定集合的结果
- Iris数据集由3种不同类型的鸢尾花（Setosa、versicolor和virginica）组成，它的花瓣和萼片长度，储存在150x4的numpy.ndarray中。
- 读取数据

- 读取数据、查看数据类型、数据形状以及基本信息

```python {cmd="G:\\Anaconda3\\python.exe" output='html'}
# import Dataset to play with it
import pandas as pd
dataset = pd.read_csv('../data/Iris.csv')
print(type(dataset))
print(dataset.shape)
print(dataset.size)
print(dataset.info())
```

- 数据内容
```python {cmd="G:\\Anaconda3\\python.exe" output='html'}
import pandas as pd
dataset = pd.read_csv('../data/Iris.csv')
print(dataset['Species'].unique())
print(dataset["Species"].value_counts())
```

- 数据样例
```python {cmd="G:\\Anaconda3\\python.exe" output='html'}
import pandas as pd
dataset = pd.read_csv('../data/Iris.csv')
print(dataset.head(5))   #开头
print(dataset.tail())    #结尾
print(dataset.sample(5)) #随机
```

- 数据描述
```python {cmd="G:\\Anaconda3\\python.exe" output='html'}
import pandas as pd
dataset = pd.read_csv('../data/Iris.csv')
print(dataset.describe()) 
```

- 数据操作
```python {cmd="G:\\Anaconda3\\python.exe" output='html'}
import pandas as pd
dataset = pd.read_csv('../data/Iris.csv')
print(dataset.where(dataset ['Species']=='Iris-setosa'))
print(dataset[dataset['SepalLengthCm']>7.2])
```

### 6.2 可视化
- IRIS的可视化技术
    - 数据可视化是用图形或图形格式表示数据。它使决策者能够直观地看到分析，这样他们就能掌握困难的概念或识别新的模式。通过交互式可视化，可以更进一步地使用技术，深入到图表和图表中，以获得更详细的信息，在这一节中将展示了多个使用matplotlib包绘制的图

#### 6.2.1 散点图
```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
dataset = pd.read_csv('../data/Iris.csv')
import seaborn as sns
import matplotlib.pyplot as plt
# Modify the graph above by assigning each species an individual color.
sns.FacetGrid(dataset, hue="Species", size=5) \
   .map(plt.scatter, "SepalLengthCm", "SepalWidthCm") \
   .add_legend()
plt.show()
```

该函数详细说明参考：http://seaborn.pydata.org/generated/seaborn.FacetGrid.map.html#seaborn.FacetGrid.map

#### 6.2.2 盒图
- 在描述性统计中，一个箱形图或箱线图是一种通过它们的四分位图来图形化地描述数字数据组的方法。
- 箱形图也可能有从盒子（胡须）垂直延伸的线，表示在上和下四分位之外的变化，因此术语盒须图和盒须图。

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
dataset = pd.read_csv('../data/Iris.csv')
dataset.plot(kind='box', subplots=True, layout=(2,3), sharex=False, sharey=False)
plt.show()
```

该函数详细说明参考：http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.html?highlight=plot#pandas.DataFrame.plot

#### 6.2.2 盒图(附)

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
sns.boxplot(x="Species", y="PetalLengthCm", data=dataset )
plt.show()
```
```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
ax= sns.boxplot(x="Species", y="PetalLengthCm", data=dataset)
ax= sns.stripplot(x="Species", y="PetalLengthCm", data=dataset, jitter=True, edgecolor="gray")
plt.show()
```
```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
ax= sns.boxplot(x="Species", y="PetalLengthCm", data=dataset)
ax= sns.stripplot(x="Species", y="PetalLengthCm", data=dataset, jitter=True, edgecolor="gray")

boxtwo = ax.artists[2]
boxtwo.set_facecolor('red')
boxtwo.set_edgecolor('black')
boxthree=ax.artists[1]
boxthree.set_facecolor('yellow')
boxthree.set_edgecolor('black')

plt.show()
```

该函数详细说明参考：http://seaborn.pydata.org/generated/seaborn.boxplot.html#seaborn.boxplot


#### 6.2.3 条形图

我们也可以创建每个输入变量的直方图来获得分布的概念

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
dataset = pd.read_csv('../data/Iris.csv')
dataset.hist(figsize=(15,20))
plt.show()
```

看起来可能有两个输入变量有 __高斯分布__。这一点值得注意，因为我们可以使用算法来利用这个假设

该函数详细说明参考：http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.hist.html?highlight=hist#pandas.DataFrame.hist


#### 6.2.4 双变量的散点图

对角线是直方图，其它的部分是两个变量之间的散点图

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
dataset = pd.read_csv('../data/Iris.csv')
pd.plotting.scatter_matrix(dataset,figsize=(10,10))
plt.show()
```

注意一些属性对的对角分组。这表明了一种高相关性和可预测的关系。

该函数详细说明参考： http://pandas.pydata.org/pandas-docs/stable/generated/pandas.plotting.scatter_matrix.html?highlight=plotting



#### 6.2.5 小提琴图

小提琴图又称核密度图，它是结合了箱形图和核密度图的图，将箱形图和密度图用一个图来显示，因为形状很像小提琴，所以被称作小提琴图。

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
sns.violinplot(data=dataset,x="Species", y="PetalLengthCm")
plt.show()
```

该函数详细说明参考：http://seaborn.pydata.org/generated/seaborn.violinplot.html?highlight=violinplot#seaborn.violinplot


#### 6.2.6 成对图
- 可以绘制成对图

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
sns.pairplot(dataset, hue="Species")
plt.show()
```

该函数详细说明参考： http://seaborn.pydata.org/generated/seaborn.pairplot.html?highlight=pairplot#seaborn.pairplot


#### 6.2.7 kdeplot
- 核密度估计(kernel density estimation)是在概率论中用来估计未知的密度函数，属于非参数检验方法之一，用于研究单变量关系


```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
sns.FacetGrid(dataset, hue="Species", size=5).map(sns.kdeplot, "PetalLengthCm").add_legend()
plt.show()
```

该函数详细说明参考： http://seaborn.pydata.org/generated/seaborn.FacetGrid.html#seaborn.FacetGrid


#### 6.2.8 jointplot
- 拟合并绘制一个六边箱图
    - 六边箱图又名高密度散点图，如果数据点太密集，绘制散点图太过密集，六边箱图是更好的选择。

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
# Use seaborn's jointplot to make a hexagonal bin plot
#Set desired size and ratio and choose a color.
sns.jointplot(x="SepalLengthCm", y="SepalWidthCm", data=dataset, size=10,ratio=10, kind='hex',color='green')
plt.show()
```

该函数详细说明参考： http://seaborn.pydata.org/generated/seaborn.jointplot.html#seaborn.jointplot


#### 6.2.9 安德鲁斯曲线
- 安德鲁斯曲线[[2]](#2)是在高维数据中可视化结构的一种方式
    - Andrews曲线将每个样本的属性值转化为傅里叶序列的系数来创建曲线
    - 通过将每一类曲线标成不同颜色可以可视化聚类数据，属于相同类别的样本的曲线通常更加接近并构成了更大的结构

每个点 $x={x_1,x_2,…,x_d }$定义一个有限傅里叶序列:
$f(t)=\frac {x_1}{\sqrt2}+x_2  sin⁡(t)+x_3  cos⁡(t)+x_4  sin⁡(2t)+ x_5  cos⁡(2t)+ …$

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
from pandas.plotting import andrews_curves
andrews_curves(dataset.drop("Id", axis=1), "Species",colormap='rainbow')
plt.show()
```
原理参考： http://www.jucs.org/jucs_11_11/visualization_of_high_dimensional/jucs_11_11_1806_1819_garc_a_osorio.pdf

该函数详细说明参考： http://pandas.pydata.org/pandas-docs/stable/generated/pandas.plotting.andrews_curves.html?highlight=andrews_curves


#### 6.2.10 热图

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
sns.heatmap(dataset.corr(),annot=True,cmap='cubehelix_r') #draws  heatmap with input as the correlation matrix calculted by(iris.corr())
plt.show()
```
该函数详细说明参考： http://seaborn.pydata.org/generated/seaborn.heatmap.html?highlight=heatmap#seaborn.heatmap


#### 6.2.11 雷达图

- 绘制雷达图
- radviz图也是一种多维的可视化图。它是基于基本的弹簧压力最小化算法，它将数据集的特征映射到二维目标空间单位圆中的一个点，点的位置由系在点上的特征决定。将实例投入到圆的中心，特征会朝园中此实例的位置（实例对应的归一化数值）“拉”实例


```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
dataset = pd.read_csv('../data/Iris.csv')
from pandas.tools.plotting import radviz
radviz(dataset.drop("Id", axis=1), "Species")
plt.show()
```
该函数详细说明参考： http://pandas.pydata.org/pandas-docs/stable/generated/pandas.plotting.radviz.html?highlight=radviz#pandas.plotting.radviz


### 6.3 数据预处理
- 数据预处理是指在将数据输入到算法之前应用于我们的数据的转换，是一种用于将原始数据转换为规整的数据集的技术，数据预处理有很多步骤，这里我们只列出了其中的一些
    - 移除目标列（id）
    - 部分鸢尾花不平衡和平衡（采样不足）
    - 引入缺失的值并对其进行处理（以平均值替换）
    - 噪声过滤
    - 数据离散化标准化和标准化
    - PCA分析特性选择


### 6.4 数据清洗

- 数据清理的主要目标
    - 检测和消除错误和异常
    - 增加分析和决策中的数据的价值

- 需要解决问题包括
    - 缺失值估算
    - 异常值检测

## 7. [模型探索](#目录)

- 在本节中，应用了 __将近20个学习算法__

- 分类算法的评价术语

    - 1) True positives(TP):  被正确地划分为正例的个数，即实际为正例且被分类器划分为正例的实例数（样本数）；
    - 2) False positives(FP): 被错误地划分为正例的个数，即实际为负例但被分类器划分为正例的实例数；
    - 3) False negatives(FN):被错误地划分为负例的个数，即实际为正例但被分类器划分为负例的实例数；
    - 4) True negatives(TN): 被正确地划分为负例的个数，即实际为负例且被分类器划分为负例的实例数。　

- 分类算法的评价指标
    - 1) 正确率（accuracy）
    正确率是我们最常见的评价指标，accuracy = (TP+TN)/(P+N)，正确率是被分对的样本数在所有样本数中的占比，通常来说，正确率越高，分类器越好。
    - 2) 错误率（error rate)
    错误率则与正确率相反，描述被分类器错分的比例，error rate = (FP+FN)/(P+N)，对某一个实例来说，分对与分错是互斥事件，所以accuracy =1 -  error rate。
    - 3) 灵敏度（sensitive）
    sensitive = TP/P，表示的是所有正例中被分对的比例，衡量了分类器对正例的识别能力。
    - 4) 特效度（specificity)
    specificity = TN/N，表示的是所有负例中被分对的比例，衡量了分类器对负例的识别能力。
    - 5) 精度（precision）
    精度是精确性的度量，表示被分为正例的示例中实际为正例的比例，precision=TP/(TP+FP)。
    - 6) 召回率（recall）
    召回率是覆盖面的度量，度量有多个正例被分为正例，recall=TP/(TP+FN)=TP/P=sensitive，可以看到召回率与灵敏度是一样的。
    - 7) 其他评价指标
    计算速度：分类器训练和预测需要的时间；
    鲁棒性：处理缺失值和异常值的能力；
    可扩展性：处理大数据集的能力；
    可解释性：分类器的预测标准的可理解性，像决策树产生的规则就是很容易理解的，而神经网络的一堆参数就不好理解，我们只好把它看成一个黑盒子。
    - 8) 查准率和查全率反映了分类器分类性能的两个方面。如果综合考虑查准率与查全率，可以得到新的评价指标F1测试值，也称为综合分类率：$F1=\frac{2 \times precision \times recall}{precision + recall}$
     为了综合多个类别的分类情况，评测系统整体性能，经常采用的还有微平均F1（micro-averaging）和宏平均F1（macro-averaging ）两种指标。宏平均F1与微平均F1是以两种不同的平均方式求的全局的F1指标。其中宏平均F1的计算方法先对每个类别单独计算F1值，再取这些F1值的算术平均值作为全局指标。而微平均F1的计算方法是先累加计算各个类别的a、b、c、d的值，再由这些值求出F1值。由两种平均F1的计算方式不难看出，宏平均F1平等对待每一个类别，所以它的值主要受到稀有类别的影响，而微平均F1平等考虑文档集中的每一个文档，所以它的值受到常见类别的影响比较大。

### 7.1 K近邻算法
- k-近邻算法(k-NN)是一种用于分类和回归的机器学习方法
- k-近邻算法代码实验如下

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# K-Nearest Neighbours
from sklearn.neighbors import KNeighborsClassifier

Model = KNeighborsClassifier(n_neighbors=8)
Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score

print('accuracy is',accuracy_score(y_pred,y_test))
```

K近邻法算法原理参考：https://www.cnblogs.com/pinard/p/6061661.html


### 7.2 限定半径最近邻算法

- 在给定半径内实现邻居投票的分类器
- 有时候我们会遇到这样的问题，即样本中某系类别的样本非常的少，甚至少于K，这导致稀有类别样本在找K个最近邻的时候，会把距离其实较远的其他样本考虑进来，而导致预测不准确。为了解决这个问题，我们限定最近邻的一个最大距离，也就是说，我们只在一个距离范围内搜索所有的最近邻，这避免了上述问题。这个距离我们一般称为限定半径。

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
from sklearn.neighbors import  RadiusNeighborsClassifier
Model=RadiusNeighborsClassifier(radius=8.0)
Model.fit(X_train,y_train)
y_pred=Model.predict(X_test)
#summary of the predictions made by the classifier
print(classification_report(y_test,y_pred))
print(confusion_matrix(y_test,y_pred))
#Accouracy score
print('accuracy is ', accuracy_score(y_test,y_pred))
```

K近邻法和限定半径最近邻法类库参数 https://www.cnblogs.com/pinard/p/6065607.html

### 7.3 逻辑回归(线性模型)
- 逻辑回归是在因变量为二分类(二元)时进行的合适的回归分析。像所有的回归分析一样，逻辑回归是预测分析

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# LogisticRegression
from sklearn.linear_model import LogisticRegression
Model = LogisticRegression()
Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score
print('accuracy is',accuracy_score(y_pred,y_test))
```

逻辑回归算法原理参考：https://www.cnblogs.com/pinard/p/6029432.html

### 7.4 被动攻击分类 (线性模型)
- Passive Aggressive Classifier实验

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
from sklearn.linear_model import PassiveAggressiveClassifier
Model = PassiveAggressiveClassifier()
Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score
print('accuracy is',accuracy_score(y_pred,y_test))
```

被动攻击算法原理参考：http://scikit-learn.org/0.19/modules/linear_model.html#passive-aggressive


### 7.5 朴素贝叶斯

在机器学习中，朴素贝叶斯分类器是一组简单的“概率分类器”，基于贝叶斯定理，特征之间有很强的(朴素的)独立性假设，下面是高斯朴素贝叶斯实验：
GaussianNB假设特征的先验概率为正态分布，即如下式：
$$
P(x_i \mid y) = \frac{1}{\sqrt{2\pi\sigma^2_y}} \exp\left(-\frac{(x_i - \mu_y)^2}{2\sigma^2_y}\right)
$$

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# Naive Bayes
from sklearn.naive_bayes import GaussianNB
Model = GaussianNB()
Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score
print('accuracy is',accuracy_score(y_pred,y_test))
```


朴素贝叶斯算法原理参考：https://www.cnblogs.com/pinard/p/6069267.html

### 7.6 多项式朴素贝叶斯分类器
- 多项式朴素贝叶斯分类器实验
- MultinomialNB假设特征的先验概率为多项式分布，即如下式：
$$
\hat{\theta}_{yi} = \frac{ N_{yi} + \alpha}{N_y + \alpha n}
$$

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# MultinomialNB
from sklearn.naive_bayes import MultinomialNB
Model = MultinomialNB()
Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score
print('accuracy is',accuracy_score(y_pred,y_test))
```

### 7.7 伯努利朴素贝叶斯分类器 
- 伯努利朴素贝叶斯分类器 实验
- BernoulliNB假设特征的先验概率为二元伯努利分布，即如下式：

$P(x_i \mid y) = P(i \mid y) x_i + (1 - P(i \mid y)) (1 - x_i)$

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# BernoulliNB
from sklearn.naive_bayes import BernoulliNB
Model = BernoulliNB()
Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score
print('accuracy is',accuracy_score(y_pred,y_test))
```

### 朴素贝叶斯类算法小结
- 这三个类适用的分类场景各不相同
    - 如果样本特征的分布大部分是连续值，使用GaussianNB会比较好。
    - 如果如果样本特征的分大部分是多元离散值，使用MultinomialNB比较合适。
    - 如果样本特征是二元离散值或者很稀疏的多元离散值，应该使用BernoulliNB

### 7.8 支持向量机 
- 基本支持向量机实验
```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# Support Vector Machine
from sklearn.svm import SVC

Model = SVC()
Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score

print('accuracy is',accuracy_score(y_pred,y_test))
```

支持向量机算法原理参考：http://www.cnblogs.com/pinard/p/6097604.html

### 7.9 Nu-Support Vector Classification
- Nu支持向量机实验

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# Support Vector Machine's 
from sklearn.svm import NuSVC

Model = NuSVC()
Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score

print('accuracy is',accuracy_score(y_pred,y_test))
```

### 7.10 线性支持向量机

- 线性支持向量机实验

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# Linear Support Vector Classification
from sklearn.svm import LinearSVC

Model = LinearSVC()
Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score

print('accuracy is',accuracy_score(y_pred,y_test))
```

###  支持向量机小结
- 支持向量机的优点是:
    - 在高维空间中有效
    - 在维度数量大于样本数量的情况下仍然有效
    - 可以为决策函数指定不同的核函数。提供了通用的核，但也可以指定定制的核
- 支持向量机的缺点是:
    - 如果特征数远大于样本数，则在选择核函数时避免过拟合，正则项至关重要

### 7.11 决策树
- 决策树算法在机器学习中算是很经典的一个算法系列了。它既可以作为分类算法，也可以作为回归算法，同时也特别适合集成学习比如随机森林。
- 决策树是一种用于分类和回归的非参数监督学习方法。目标是创建一个模型，通过学习从数据特性推断出的简单决策规则来预测目标变量的值

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# Decision Tree's
from sklearn.tree import DecisionTreeClassifier

Model = DecisionTreeClassifier()

Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score
print('accuracy is',accuracy_score(y_pred,y_test))
```

决策树算法原理参考：https://www.cnblogs.com/pinard/p/6050306.html


### 7.12 额外决策树
- 额外树与传统决策树的不同之处在于它们的构建方式。在寻找最好的分割方法将一个节点的样本分成两组时，对每个随机选择的max_features绘制随机分割，并从中选择最好的分割。当max_features设置为1时，这相当于构建了一个完全随机的决策树。


```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
# ExtraTreeClassifier
from sklearn.tree import ExtraTreeClassifier

Model = ExtraTreeClassifier()

Model.fit(X_train, y_train)

y_pred = Model.predict(X_test)

# Summary of the predictions made by the classifier
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
# Accuracy score
print('accuracy is',accuracy_score(y_pred,y_test))
```


### 决策树小节

- 决策树算法的优点：
    - 简单直观，生成的决策树很直观
    - 基本不需要预处理，不需要提前归一化，处理缺失值
    - 使用决策树预测的代价是O(log2m)。 m为样本数
    - 既可以处理离散值也可以处理连续值。很多算法只是专注于离散值或者连续值
    - 可以处理多维度输出的分类问题
    - 相比于神经网络之类的黑盒分类模型，决策树在逻辑上可以得到很好的解释
- 我们再看看决策树算法的缺点:
    - 决策树算法非常容易过拟合，导致泛化能力不强。可以通过设置节点最少样本数量和限制决策树深度来改进
    - 决策树会因为样本发生一点点的改动，就会导致树结构的剧烈改变。这个可以通过集成学习之类的方法解决
    - 寻找最优的决策树是一个NP难的问题，我们一般是通过启发式方法，容易陷入局部最优。可以通过集成学习之类的方法来改善
    - 有些比较复杂的关系，决策树很难学习，比如异或。这个就没有办法了，一般这种关系可以换神经网络分类方法来解决


### 7.13 神经网络
- 即多层感知器分类器
- 模型采用随机梯度下降等方法对对数损失函数进行优化


```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
from sklearn.neural_network import MLPClassifier
Model=MLPClassifier()
Model.fit(X_train,y_train)
y_pred=Model.predict(X_test)
# Summary of the predictions
print(classification_report(y_test,y_pred))
print(confusion_matrix(y_test,y_pred))
#Accuracy Score
print('accuracy is ',accuracy_score(y_pred,y_test))
```

该函数详细说明参考：http://scikit-learn.org/stable/modules/generated/sklearn.neural_network.MLPClassifier.html#sklearn.neural_network.MLPClassifier


### 集成学习概述

- 集成学习之boosting
    - Boosting算法的工作机制是首先从训练集用初始权重训练出一个弱学习器1，根据弱学习的学习误差率表现来更新训练样本的权重，使得之前弱学习器1学习误差率高的训练样本点的权重变高，使得这些误差率高的点在后面的弱学习器2中得到更多的重视。然后基于调整权重后的训练集来训练弱学习器2.，如此重复进行，直到弱学习器数达到事先指定的数目T，最终将这T个弱学习器通过集合策略进行整合，得到最终的强学习器（AdaBoost、GDBT）
- 集成学习之bagging
    - Bagging的算法原理和 boosting不同，它的弱学习器之间没有依赖关系，可以并行生成（随机森林、bagging）


### 7.14 随机森林
- 随机森林是一种元估计，它适用于数据集的各种子样本上的许多决策树分类器，并使用平均法来提高预测精度和控制过拟合。子样本大小总是与原始输入样本大小相同，但是如果bootstrap=True(默认)，则用替换的方式绘制样本。

- 随机森林的实验

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
from sklearn.ensemble import RandomForestClassifier
Model=RandomForestClassifier(max_depth=2)
Model.fit(X_train,y_train)
y_pred=Model.predict(X_test)
print(classification_report(y_test,y_pred))
print(confusion_matrix(y_pred,y_test))
#Accuracy Score
print('accuracy is ',accuracy_score(y_pred,y_test))
```

随机森林算法原理参考：https://www.cnblogs.com/pinard/p/6156009.html


### 7.15 Bagging
- bagging分类器是一个集成元估计器，它在原始数据集的随机子集上适合每个基本分类器，然后汇总它们的单个预测(通过投票或平均)，形成最终的预测。这种元估计器通常可以作为一种方法来减少黑盒估计器(例如，决策树)的方差，方法是在构建过程中引入随机性，然后从中生成一个集成。这个算法包含了文献中的一些作品。当数据集的随机子集作为样本的随机子集绘制时，这种算法称为粘贴。如果样品是用替换法绘制的，那么这种方法被称为套袋法。当将数据集的随机子集绘制为特征的随机子集时，该方法称为随机子空间。最后，当基估计量是建立在样本和特征的子集上时，这种方法被称为随机斑块
- Bagging的实验

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
from sklearn.ensemble import BaggingClassifier
Model=BaggingClassifier()
Model.fit(X_train,y_train)
y_pred=Model.predict(X_test)
print(classification_report(y_test,y_pred))
print(confusion_matrix(y_pred,y_test))
#Accuracy Score
print('accuracy is ',accuracy_score(y_pred,y_test))
```


Bagging算法原理参考：https://www.cnblogs.com/pinard/p/6156009.html


### 7.16 自适应提示分类器Adaboost
- AdaBoost分类器是一个元估计器，它首先在原始数据集上拟合一个分类器，然后在相同的数据集上拟合分类器的其他副本，但是不正确分类实例的权重会被调整，以便后续分类器更关注困难的情况。
- 自适应提示分类器实验

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
from sklearn.ensemble import AdaBoostClassifier
Model=AdaBoostClassifier()
Model.fit(X_train,y_train)
y_pred=Model.predict(X_test)
print(classification_report(y_test,y_pred))
print(confusion_matrix(y_pred,y_test))
#Accuracy Score
print('accuracy is ',accuracy_score(y_pred,y_test))
```

Adaboost算法原理参考：https://www.cnblogs.com/pinard/p/6133937.html


### 7.17 梯度提升分类器GradientBoosting

- 梯度提升分类器（GBDT）实验

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
from sklearn.ensemble import GradientBoostingClassifier
Model=GradientBoostingClassifier()
Model.fit(X_train,y_train)
y_pred=Model.predict(X_test)
print(classification_report(y_test,y_pred))
print(confusion_matrix(y_pred,y_test))
#Accuracy Score
print('accuracy is ',accuracy_score(y_pred,y_test))
```


梯度提升树(GBDT)算法原理参考：https://www.cnblogs.com/pinard/p/6140514.html


### 7.18 线性判别分析LDA
- 一线性判别分析(discriminant_analysis.LinearDiscriminantAnalysis)和二次判别分析(discriminant_analysisquadraticdiscriminantanalysis)是两种经典的分类方法，顾名思义，它们分别是一个线性和一个二次决策曲面。这些分类器很有吸引力，因为它们有可以容易计算的封闭形式的解决方案，本质上是多类的，已经被证明在实践中工作得很好，并且没有超参数可调
- LDA实验

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis
Model=LinearDiscriminantAnalysis()
Model.fit(X_train,y_train)
y_pred=Model.predict(X_test)
print(classification_report(y_test,y_pred))
print(confusion_matrix(y_pred,y_test))
#Accuracy Score
print('accuracy is ',accuracy_score(y_pred,y_test))
```

线性判别分析LDA算法原理参考：https://www.cnblogs.com/pinard/p/6244265.html


### 7.19 二次判别分析QDA
- 一种具有二次决策边界的分类器，它是利用贝叶斯规则将类条件密度拟合到数据上而产生的。该模型适用于每个类的高斯密度
- QDA实验

```python {cmd="G:\\Anaconda3\\python.exe" output='html' matplotlib=true}
import pandas as pd
#进行评价需要导入的包
from sklearn.metrics import classification_report
from sklearn.metrics import confusion_matrix
from sklearn.metrics import accuracy_score
dataset = pd.read_csv('../data/Iris.csv')
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, -1].values
# Splitting the dataset into the Training set and Test set
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size = 0.2, random_state = 0)
from sklearn.discriminant_analysis import QuadraticDiscriminantAnalysis
Model=QuadraticDiscriminantAnalysis()
Model.fit(X_train,y_train)
y_pred=Model.predict(X_test)
print(classification_report(y_test,y_pred))
print(confusion_matrix(y_pred,y_test))
#Accuracy Score
print('accuracy is ',accuracy_score(y_pred,y_test))
```

二次判别分析QDA算法原理参考：http://scikit-learn.org/stable/modules/lda_qda.html#dimensionality-reduction-using-linear-discriminant-analysis


## 8. [总结](#目录)

- 本篇博客中，用各种Python包覆盖ML过程中所有相关的部分
- 实际Kaggle比赛使用的方法
    - Gradient Boosting 本身优秀的性能加上 Xgboost 高效的实现，使得它在 Kaggle 上广为使用
    - 几乎每场比赛的获奖者都会用 Xgboost 作为最终 Model 的重要组成部分
    - 在实战中，往往会以 Xgboost 为主来建立我们的模型并且验证 Feature 的有效性
- 实践代码简述与思考，详见：https://github.com/Dongzhixiao/Python_Exercise/tree/master/test20MachineLearning



## 9. [参考文献](#目录)

<a name="1" target="_blank" href="http://pdfs.semanticscholar.org/ab21/376e43ac90a4eafd14f0f02a0c87502b6bbf.pdf">[1] Fisher R A. THE USE OF MULTIPLE MEASUREMENTS IN TAXONOMIC PROBLEMS[J]. Annals of Human Genetics, 2012, 7(2):179-188.</a>

<a name="2" target="_blank" href="https://www.researchgate.net/publication/242529927_Plots_of_High-Dimensional_Data">[2] Andrews D F. Plots of High-Dimensional Data[J]. Biometrics, 1972, 28(1):125-136.</a>

