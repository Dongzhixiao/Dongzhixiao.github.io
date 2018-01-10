---
layout:     post
title:      "使用机器学习和数据挖掘算法进行数据处理"
subtitle:   "机器学习 数据挖掘 算法 数据处理"
date:       2017-12-12
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20171212.jpg?raw=true"
tags:
    - 机器学习
    - 数据挖掘
---


### 时间:2017年12月12日 天气:阴:cloud:
-----
#####   Author:冬之晓
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

# 一 介绍
数据挖掘和机器学习是进行数据处理的非常有用的工具，当代的好多数据都使用这两种方法。但是这两种方法却包含很多模型和方法，对于初学者来说，面对这些模型总是无从下手。因此，后面的论述主要以处理数据的流程入手，把每个方法带入到数据处理的步骤中来讲，使得这些方法在数据处理中的具体位置有一个清晰的显示，有利于理解这些方法。
对于数据处理来说，整个处理的流程如下图1所示[1]：
![处理的流程](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/ProceedingOfData.png?raw=true)
由此可见，数据处理的流程大体分为：
1. 数据预处理——通常包括特征选择、维规约、规范化等方法。
2. 数据挖掘——这部分的方法和技术非常多，在处理时主要根据自己的目的来选择对应的方法最为恰当。
3. 数据后处理——主要包括模式过滤、可视化等，目的是为了让数据挖掘的结果利于使用和观察。
上述介绍中，数据预处理属于数据基本信息的内容。因此，本文后面第2小结会介绍数据的相关基本技术和方法，第3小节会介绍数据挖掘和机器学习的相关技术和方法[1][2]，第4小节主要关注于后处理的技术和方法。
为了让大家有一个清晰的框架，后面内容的思维导图如下展示：
![数据挖掘与机器学习知识图谱](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/MachineLearningAndDataMining.png?raw=true)

# 二 了解数据
其实数据处理最关键的地方在于解决问题，并不是使用的方法越复杂越好。无论方法多么简单，只要解决问题就是好的方法。为了解决数据处理的相关问题，第一步是观察数据，了解数据相关的概念，然后对数据进行一些处理。这样对后面具体使用哪个方法来进行分析非常有用。
## 2.1数据预处理
数据预处理对于后续使用数据挖掘或者机器学习技术非常重要。在面临大数据的当下，数据的维度通常非常的多，因此数据预处理的一个主要任务就是降低数据维度。
### 2.1.1维归约
所谓维归约，就是要减少数据的特征数目，摒弃掉不重要的特征，尽量只用少数的关键特征来描述数据。人们总是希望看到的现象主要是由少数的关键特征造成的，找到这些关键特征也是数据分析的目的。维归约中主要方法很多，下面介绍几个：
#### （1）主成分分析
主成分分析是一种统计方法。通过正交变换将一组可能存在相关性的变量转换为一组线性不相关的变量，转换后的这组变量叫主成分。
主成分分析的基本解决的问题是：正交属性空间中的样本点，如何使用一个超平面对所有样本进行恰当的表达？若存在这样的超平面，那么它大概应具有这样的性质：
最近重构性：样本点到这个超平面的距离都足够近。
最大可分性：样本点在这个超平面上的投影能尽可能分开。
根据这两个性质即可得到主成分分析的两种等价推导。
**优点：**
1、 可消除评价指标之间的相关影响，因为主成分分析在对原指标变量进行变换后形成了彼此相互独立的主成分，而且实践证明指标之间相关程度越高，主成分分析效果越好。
2、 可减少指标选择的工作量，对于其它评价方法，由于难以消除评价指标间的相关影响，所以选择指标时要花费不少精力，而主成分分析由于可以消除这种相关影响，所以在指标选择上相对容易些。
3、 当评级指标较多时还可以在保留绝大部分信息的情况下用少数几个综合指标代替原指标。
**缺点：**
1．在主成分分析中，我们首先应保证所提取的前几个主成分的累计贡献率达到一个较高的水平（即变量降维后的信息量须保持在一个较高水平上），其次对这些被提取的主成分必须都能够给出符合实际背景和意义的解释（否则主成分将空有信息量而无实际含义）。 
2．主成分的解释其含义一般多少带有点模糊性，不像原始变量的含义那么清楚、确切，这是变量降维过程中不得不付出的代价。因此，提取的主成分个数m通常应明显小于原始变量个数p（除非p本身较小），否则维数降低的“利”可能抵不过主成分含义不如原始变量清楚的“弊”。
3．只能处理线性降维。
#### （2）核主成分分析
线性降维方法假设从高维空间到低维空间的函数映射是线性的，然而在不少现实任务中，可能需要非线性映射才能找到恰当的低维嵌入。如果直接使用线性降维的方式，有可能使得数据丧失原有的低维结构。非线性降维的一种常用方法是，基于核技巧对线性降维方法进行“核化”。
#### （3）流形学习
流形学习是一类借鉴了拓扑流形概念的降维方法。“流形”是在局部与欧氏空间同胚的空间，换言之，它在局部具有欧氏空间的性质，能用欧氏距离进行距离的计算。这给降维方法带来了启发：若低维流形嵌入到高维空间，则数据样本在高维空间中的分布虽然比较复杂，但在局部上仍具有欧氏空间的性质。因此，可以容易的在局部建立降维映射关系，然后设法将局部映射关系推广到全局。
#### （4）多维缩放(Multiple Dimensional Scaling, MDS)
多维缩放是一种降维方法，要求原始空间中样本之间的距离在低维空间中得以保持。
### 2.1.2特征选择
特征选择( Feature Selection )也称特征子集选择( Feature Subset Selection , FSS )，或属性选择( Attribute Selection )。是指从已有的M个特征(Feature)中选择N个特征使得系统的特定指标最优化，是从原始特征中选择出一些最有效特征以降低数据集维度的过程,是提高学习算法性能的一个重要手段,也是模式识别中关键的数据预处理步骤。对于一个学习算法来说，好的学习样本是训练模型的关键。
假设原始特征集中有n个特征（也称输入变量），那么存在2^(n-1)个可能的非空特征子集。搜索策略就是为了从包含2^(n-1)个候选解的搜索空间中寻找最优特征子集而采取的搜索方法。搜索策略可大致分为以下3类：
#### （1）过滤式
Relief(Relevant Features)算法
#### （2）包裹式
LVW(Las Vegas Wrapper)算法
#### （3）嵌入式
L1/L2 正则化
**思考：**
特征选择也是一种降维方式，但是它处理的方式和主成分分析有区别，它直接删除了某些特征。而主成分分析的每个特征都是其他特征的线性组合。
## 2.2相似性衡量
在做分类或者聚类任务时常常需要估算不同样本之间的相似性度量(Similarity Measurement)，这时通常采用的方法就是计算样本间的“距离”(Distance)。采用什么样的方法计算距离是很讲究，甚至关系到分类的正确与否。下面就是对常用的相似性度量作一个总结。
### 2.2.1 闵可夫斯基距离
闵氏距离不是一种距离，而是一组距离的定义。
$$dist=(\sum_{k=1}^{n}|p_k-q_k|^r)^{\frac{1}{r}}$$
其中r是一个变参数。
当r=1时，就是曼哈顿距离。
当r=2时，就是欧氏距离。
当r→∞时，就是切比雪夫距离。
根据变参数的不同，闵氏距离可以表示一类的距离。
闵氏距离，包括曼哈顿距离、欧氏距离和切比雪夫距离都存在明显的缺点。
简单说来，闵氏距离的**缺点**主要有两个：
(1) 将各个分量的量纲(scale)，也就是“单位”当作相同的看待了。
(2) 没有考虑各个分量的分布（期望，方差等)可能是不同的。
### 2.2.2 标准化欧氏距离
标准化欧氏距离是针对简单欧氏距离的缺点而作的一种改进方案。标准欧氏距离的思路：数据各维分量的分布不一样，因此先将各个分量都“标准化”到均值、方差相等。

如果将方差的倒数看成是一个权重，这个公式可以看成是一种加权欧氏距离(Weighted Euclidean distance)。
### 2.2.3 马氏距离
马氏距离的优缺点：量纲无关，排除变量之间的相关性的干扰。
###2.2.4 夹角余弦
几何中夹角余弦可用来衡量两个向量方向的差异，机器学习中借用这一概念来衡量样本向量之间的差异。
### 2.2.5 汉明距离
两个等长字符串s1与s2之间的汉明距离定义为将其中一个变为另外一个所需要作的最小替换次数。例如字符串“1111”与“1001”之间的汉明距离为2。
应用：信息编码（为了增强容错性，应使得编码间的最小汉明距离尽可能大）。
### 2.2.6 杰卡德距离&杰卡德相似系数
### 2.2.7 相关系数&相关距离
## 2.3 度量学习
在机器学习中，对高维数据进行降维的主要目标是希望找到一个合适的低维空间，在此空间中进行学习能比原始空间性能更好。事实上，每个空间对应了在样本属性上定义的一个距离度量，而寻找合适的空间，本质上就是在寻找一个合适的距离度量。因此，度量学习提出了直接学习出一个合适的距离度量的方案。
# 三 数据挖掘与机器学习
在进行完数据探索和预处理后，可能需要对自己的目标数据选择具体的方法来进行进一步分析。数据探索的动机通常是对数据进行分类、聚类和关联分析以及异常检测，因此下面的方法和技术按照这个顺序介绍。
## 3.1分类
分类的方法多种多样，具体来说，有下面介绍的主要方法。
### 3.1.1决策树
决策树（decision tree）是一个树结构（可以是二叉树或非二叉树）。其每个非叶节点表示一个特征属性上的测试，每个分支代表这个特征属性在某个值域上的输出，而每个叶节点存放一个类别。使用决策树进行决策的过程就是从根节点开始，测试待分类项中相应的特征属性，并按照其值选择输出分支，直到到达叶子节点，将叶子节点存放的类别作为决策结果。决策树在选择分支属性的时候有不同的方案：
####（1）著名的ID3决策树主要根据“信息增益(information gain)”来进行划分属性选择。
####（2）著名的C4.5决策树主要根据“增益率”来进行划分属性选择。
后续还有“减枝”处理和“缺失值”的相关处理。目前决策树已经成功运用于医学、制造产业、天文学、分支生物学以及商业等诸多领域。
**思考：**决策树的决策过程非常直观，容易被人理解，因此其除了可以用于分类外，也可用于可视化，因为决策树的结果很容易做成图，结果比较清晰。
### 3.1.2 基于规则的分类器
基于规则的分类器是使用一组“if…then…”规则来对记录进行分类的技术。
**思考：**基于规则的分类器是最好有创新点的分类方法，因为规则都是自己定的。
### 3.1.3最近邻分类器
k近邻法(k-nearest neighbor, k-NN)是1967年由Cover T和Hart P提出的一种基本分类与回归方法。它的工作原理是：存在一个样本数据集合，也称作为训练样本集，并且样本集中每个数据都存在标签，即我们知道样本集中每一个数据与所属分类的对应关系。输入没有标签的新数据后，将新的数据的每个特征与样本集中数据对应的特征进行比较，然后算法提取样本最相似数据(最近邻)的分类标签。一般来说，我们只选择样本数据集中前k个最相似的数据，这就是k-近邻算法中k的出处，通常k是不大于20的整数。最后，选择k个最相似数据中出现次数最多的分类，作为新数据的分类。
### 3.1.4 贝叶斯分类器
#### （1）朴素贝叶斯分类器
朴素贝叶斯分类器是一个简单的概率分类器。为避免直接计算x的联合概率分布，朴素贝叶斯分类器采用了“属性条件独立性假设”,对已知类别，假设所有属性互相独立。
**优点：**实现简单。
**缺点：**使用条件苛刻，必须假定属性直接条件独立，而且必须实现知道属性的概率分布。
#### （2）半朴素贝叶斯分类器
在朴素贝叶斯分类器中，有一个重要假设就是属性条件独立性。然而，在实际情况下，这个假设往往很难成立。从对属性条件独立性进行一定程度的放松出发，产生了一类称为“半朴素贝叶斯分类器”的学习方法。 
半朴素贝叶斯分类器的基本思路就是适当考虑一部分属性间的相互依赖信息，比如“独依赖估计”是半朴素贝叶斯分类器最常用的一种方法。“独依赖”的意思是假设每个属性在类别之外最多依赖于一个其他属性。
#### （3）贝叶斯网
贝叶斯网亦称为“信念网”，它借助有向无环图来刻画属性之间的依赖关系，并使用条件概率表来描述属性的联合概率分布。一个贝叶斯网由结构G和参数Θ构成，即B=⟨G,Θ⟩。网络结构G是一个有向无环图，每个结点对应于一个属性，若两个属性有直接依赖关系，则它们由一条边连接起来；参数Θ定量描述这种依赖关系，假设属性xi在G中的父节点集为πi，则Θ包含了每个属性的条件概率表θxi|πi=PB(xi|πi)。
**优点：**考虑了所有的属性之间的相关性。
**缺点：**实现较为复杂。
### 3.1.5 神经网络
神经网络是由具有适应性的简单单元组成的广泛并行互联的网络，它的组织能够模拟生物神经系统对真实世界物体所作出的交叉反应”。 
神经网络中最基本的成分是神经元模型。在生物神经网络中，每个神经元与其他神经元相连，当它“兴奋”时，就会向相连的神经元发送化学物质，从而改变神经元的电位；如果神经元的电位超过了一个阈值，它就会被激活，即“兴奋”起来，向其他神经元发送化学物质。
理想的激活函数应该是阶跃函数。但阶跃函数不连续且不光滑。在实践中，多采用Sigmoid函数作为激活函数。为了训练多层网络，目前最好的算法是误差逆传播（BP）算法，它是迄今最成功的神经网络学习算法。
常见的神经网络有：
#### （1）RBF网络
RBF（Radial Basis Function，径向基函数）网络是一种单隐层前馈神经网络，它使用径向基函数作为隐层神经单元激活函数，而输出层是对隐层神经元输出的线性组合。径向基函数是沿径向对称的标量函数，通常定义为样本x到数据中心c之间的欧氏距离的单调函数。
#### （2）ART网络
竞争性学习是神经网络中一种常用的无监督学习策略，在使用该策略时，网络的输出神经元互相竞争，每一时刻仅有一个竞争获胜的神经元被激活，其他神经元的状态被抑制。 
ART（Adaptive Resonance Theory, 自适应谐振理论）网络是竞争性学习的最佳代表，该网络由比较层、识别层、识别阈值和重置模块构成。其中，比较层负债接受输入样本，并将其传输给识别层神经元。识别层每个神经元对应一个模式类，神经元数目可在训练过程中动态增加以增加新的模式类。通过这种方式，ART网络兼具了可塑性与稳定性，可进行增量学习或在线学习。
#### （3）SOM网络
SOM（Self-Organizing Map, 自映射）网络，是一种竞争学习型的无监督神经网络，它能将高维输入数据映射到低维空间（通常为二维）,同时保持输入数据在高维空间中的拓扑结构，即将高维空间中相似的样本映射到网络输出层中的邻近神经元。 
SOM网络中的输出层神经元以矩阵方式排列在二维空间中，每个神经元都拥有一个权向量。网络在接受输入向量后，将会确定输出层获胜神经元，它决定了该输入向量在低维空间中的位置。SOM的训练目标是为每个输出层神经元找到合适的权向量，以达到保持拓扑结构的目的。
#### （4）级联相关网络
一般的神经网络通常假定结构是固定的，而结构自适应网络则将网络结构也作为学习的目标之一，并希望能在数据训练过程中找到最符合数据特点的网络结构。级联相关（Cascade-Correlation）网络是结构自适应网络的重要代表。 
在开始训练时，网络只有输入层和输出层，处于最小拓扑结构。随着训练的进行，新的隐层神经元逐渐加入，从而创建起层级结构。当新的隐层神经元加入时，输入端连接权值时冻结的，通过最大化新神经元的输出与网络误差之间的相关性来训练相关参数。
#### （5）Elman网络
递归神经网络(recurrent neutral networks)允许网络中出现环形结构，从而可让一些神经元的输出反馈回来作为输入信号。这样的反馈过程，使得网络能够处理与时间有关的动态变化。 
Elman网络是最常用的递归神经网络之一。网络的训练采用推广的BP算法
#### （6）Boltzman机
Boltzman机是一种“基于能量的模型”。
#### （7）深度学习
典型的深度学习就是深层的神经网络。在训练时，多隐层（三个以上隐层）神经网络难以直接用BP算法进行训练，因为误差在多隐层内逆传播时，往往会“发散”而不能收敛到温度状态。有两种方式可以在训练模型的同时节省训练开销： 
##### 1) 无监督逐层训练。这是多隐层网络训练的有效手段。基本思想是每次训练一层隐节点，训练时将上一层隐节点的输出作为输入，本层隐节点的输出作为下一层隐节点的输入。这被称为“预训练”。在预训练全部完成后，再对整个网络进行“微调”。这种方式在深度信念网络（deep belief network，DBN）中得到应用。 
##### 2) 权共享。让一组神经元使用相同的连接权。这种方式在卷积神经网络（Convolutional Neutral Network,CNN）中得到应用。
**思考：**神经网络，包括其后面的深度学习是现在研究的热点，因为其结果通常很好，但是其本质是一个“黑箱模型”，因此可解释性不好。
### 3.1.6支持向量机
## 3.2聚类
聚类也是一个非常大的主题。在“无监督学习”任务中研究最多、应用最广，目标是将数据样本划分为若干个通常不相交的“簇”(cluster)。既可以作为一个单独过程（用于找寻数据内在的分布结构），也可作为分类等其他学习任务的前驱过程。
聚类首先必须了解聚类相关的相似衡量（见2.2小结），聚类的基本想法是：“簇内相似度”（intra-cluster similarity）高，且“簇间相似度”（inter-cluster similarity）低。在这种思想的指导下，可以使用多种方法根据不同的相似衡量进行聚类，具体有以下几类。
### 3.2.1划分聚类
划分聚类中最典型的算法就是K-means聚类算法。基于原型的、划分的距离技术，它试图发现用户指定个数(K)的簇。算法思想较为简单如下所示：
```
选择K个点作为初始质心  
repeat  
    将每个点指派到最近的质心，形成K个簇  
    重新计算每个簇的质心  
until 簇不发生变化或达到最大迭代次数  
```
这里的重新计算每个簇的质心，如何计算的是根据目标函数得来的，因此需要考虑距离度量和目标函数，即聚类相关的相似衡量（见2.2小结）。
**优点：**简单、时间复杂度、空间复杂度低。
**缺点：**随机初始化的中心点对结果影响很大。
### 3.2.2继承聚类
继承聚类试图在不同层次对数据集进行划分，从而形成树形的聚类结构。数据集的划分可使用“自底向上”的聚合策略，也可以采用“自顶向下”的分拆策略。 
AGNES是一种采用自底向上的聚合策略的层次聚类算法。它先将数据集中的每个样本都看做一个初始聚类簇，然后再算法运行的每一步中找出距离最近的两个聚类簇进行合并，该过程不断重复，直至达到预设的聚类簇个数。当聚类簇距离由最小距离/最大距离/平均距离计算时，AGNES算法被相应的称为“单连接”、“全连接”或“均连接”。
### 3.2.3密度聚类
密度聚类也被称为“基于密度的聚类”，这类算法假定聚类结构能通过样本分布的紧密程度确定。通常情况下，密度聚类算法从样本密度的角度来考察样本之间的可连接性，并基于可连接样本不断扩展聚类簇以获得最终的聚类结果。 
DBSCAN是一种著名的密度聚类算法，它基于一组“领域”参数来刻画样本分布的紧密程度。
### 3.2.4 基于图的聚类
还未学习，待完善
### 3.2.5可伸缩聚类算法
还未学习，待完善
## 3.3关联分析
关联分析是一种根据数据的内容标签，然后得到数据之间的关系的分析技术。在交易数据、关系数据或其他信息载体中，查找存在于项目集合或对象集合之间的频繁模式、关联、相关性或因果结构。
### 3.3.1 Apriori算法
Apriori算法是挖掘产生布尔关联规则所需频繁项集的基本算法，也是最著名的关联规则挖掘算法之一。Apriori算法就是根据有关频繁项集特性的先验知识而命名的。它使用一种称作逐层搜索的迭代方法，k—项集用于探索（k+1）—项集。首先，找出频繁1—项集的集合．记做L1，L1用于找出频繁2—项集的集合L2，再用于找出L3，如此下去，直到不能找到频繁k—项集。找每个Lk需要扫描一次数据库。
为提高按层次搜索并产生相应频繁项集的处理效率，Apriori算法利用了一个重要性质，并应用Apriori性质来帮助有效缩小频繁项集的搜索空间。
Apriori性质：一个频繁项集的任一子集也应该是频繁项集。证明根据定义，若一个项集I不满足最小支持度阈值min_sup，则`I`不是频繁的，即`P（I）<min_sup`。若增加一个项A到项集I中，则结果新项集`（I∪A）`也不是频繁的，在整个事务数据库中所出现的次数也不可能多于原项集I出现的次数，因此`P（I∪A）<min_sup`，即`（I∪A）`也不是频繁的。这样就可以根据逆反公理很容易地确定Apriori性质成立。
针对Apriori算法的不足，对其进行优化：
#### （1）基于划分的方法。
该算法先把数据库从逻辑上分成几个互不相交的块，每次单独考虑一个分块并对它生成所有的频繁项集，然后把产生的频繁项集合并，用来生成所有可能的频繁项集，最后计算这些项集的支持度。这里分块的大小选择要使得每个分块可以被放入主存，每个阶段只需被扫描一次。而算法的正确性是由每一个可能的频繁项集至少在某一个分块中是频繁项集保证的。
上面所讨论的算法是可以高度并行的。可以把每一分块分别分配给某一个处理器生成频繁项集。产生频繁项集的每一个循环结束后．处理器之间进行通信来产生全局的候选是一项集。通常这里的通信过程是算法执行时间的主要瓶颈。而另一方面，每个独立的处理器生成频繁项集的时间也是一个瓶颈。其他的方法还有在多处理器之间共享一个杂凑树来产生频繁项集，更多关于生成频繁项集的并行化方法可以在其中找到。
#### （2） 基于Hash的方法。
Park等人提出了一个高效地产生频繁项集的基于杂凑（Hash）的算法。通过实验可以发现，寻找频繁项集的主要计算是在生成频繁2—项集Lk上，Park等就是利用这个性质引入杂凑技术来改进产生频繁2—项集的方法。
#### （3） 基于采样的方法。
基于前一遍扫描得到的信息，对它详细地做组合分析，可以得到一个改进的算法，其基本思想是：先使用从数据库中抽取出来的采样得到一些在整个数据库中可能成立的规则，然后对数据库的剩余部分验证这个结果。这个算法相当简单并显著地减少了FO代价，但是一个很大的缺点就是产生的结果不精确，即存在所谓的数据扭曲（Dataskew）。分布在同一页面上的数据时常是高度相关的，不能表示整个数据库中模式的分布，由此而导致的是采样5%的交易数据所花费的代价同扫描一遍数据库相近。
#### （4） 减少交易个数。
减少用于未来扫描事务集的大小，基本原理就是当一个事务不包含长度为志的大项集时，则必然不包含长度为走k+1的大项集。从而可以将这些事务删除，在下一遍扫描中就可以减少要进行扫描的事务集的个数。这就是AprioriTid的基本思想。
### 3.3.2 FP-growth算法
由于Apriori方法的固有缺陷．即使进行了优化，其效率也仍然不能令人满意。2000年，Han Jiawei等人提出了基于频繁模式树（Frequent Pattern Tree，简称为FP-tree）的发现频繁模式的算法FP-growth。在FP-growth算法中，通过两次扫描事务数据库，把每个事务所包含的频繁项目按其支持度降序压缩存储到FP—tree中。在以后发现频繁模式的过程中，不需要再扫描事务数据库，而仅在FP-Tree中进行查找即可，并通过递归调用FP-growth的方法来直接产生频繁模式，因此在整个发现过程中也不需产生候选模式。该算法克服了Apriori算法中存在的问颢．在执行效率上也明显好于Apriori算法。
## 3.4 异常检测
还未学习，待完善
# 四、数据后处理
在分析完数据后，通常需要使用合适的后处理方法对数据的结果进行显示，其实在数据挖掘里面主要称作可视化数据挖掘。
## 4.1 数据可视化
数据可视化，是关于数据视觉表现形式的科学技术研究。其中，这种数据的视觉表现形式被定义为，一种以某种概要形式抽提出来的信息，包括相应信息单位的各种属性和变量。
它是一个处于不断演变之中的概念，其边界在不断地扩大。主要指的是技术上较为高级的技术方法，而这些技术方法允许利用图形、图像处理、计算机视觉以及用户界面，通过表达、建模以及对立体、表面、属性以及动画的显示，对数据加以可视化解释。与立体建模之类的特殊技术方法相比，数据可视化所涵盖的技术方法要广泛得多。（注意：这里虽然将数据可视化放在数据后处理小结里面，但是实际操作中，数据可视化通常也在数据预处理中使用，目的是为了找到数据之间的关系，来决定后面使用什么方法进行进一步分析。）
### 4.1.1 少量数据可视化
#### （1）茎叶图
茎叶图（Stem-and-Leaf display)又称“枝叶图”，20世纪早期由英国统计学家阿瑟•鲍利（Arthur Bowley）设计，1977年统计学家约翰托奇( John Tukey)在其著作《探索性数据分析》（exploratory data analysis）中将这种绘图方法介绍给大家，从此这种作图方法变得流行起来。它的思路是将数组中的数按位数进行比较，将数的大小基本不变或变化不大的位作为一个主干（茎），将变化大的位的数作为分枝（叶），列在主干的后面，这样就可以清楚地看到每个主干后面的几个数，每个数具体是多少。
#### （2）直方图
直方图(Histogram)又称质量分布图。是一种统计报告图，由一系列高度不等的纵向条纹或线段表示数据分布的情况。 一般用横轴表示数据类型，纵轴表示分布情况。
直方图是数值数据分布的精确图形表示。 这是一个连续变量（定量变量）的概率分布的估计，并且被卡尔•皮尔逊（Karl Pearson）首先引入。它是一种条形图。 为了构建直方图，第一步是将值的范围分段，即将整个值的范围分成一系列间隔，然后计算每个间隔中有多少值。 这些值通常被指定为连续的，不重叠的变量间隔。 间隔必须相邻，并且通常是（但不是必须的）相等的大小。
直方图也可以被归一化以显示“相对”频率。 然后，它显示了属于几个类别中的每个案例的比例，其高度等于1。
#### （4）盒状图
盒图是在1977年由美国的统计学家约翰•图基(John Tukey)发明的。它由五个数值点组成：最小值(min)，下四分位数(Q1)，中位数(median)，上四分位数(Q3)，最大值(max)。也可以往盒图里面加入平均值(mean)。下四分位数、中位数、上四分位数组成一个“带有隔间的盒子”。上四分位数到最大值之间建立一条延伸线，这个延伸线成为“胡须(whisker)”。
由于现实数据中总是存在各式各样的“离群点”，于是为了不因这些少数的离群数据导致整体特征的偏移，将这些离群点单独汇出，而盒图中的胡须的两级修改成最小观测值与最大观测值。这里有个经验，就是最大(最小)观测值设置为与四分位数值间距离为1.5个IQR(中间四分位数极差)。
#### （5）饼图
饼图英文学名为Sector Graph, 有名Pie Graph。常用于统计学模块。2D饼图为圆形，手画时，常用圆规作图。
仅排列在工作表的一列或一行中的数据可以绘制到饼图中。饼图显示一个数据系列 （数据系列：在图表中绘制的相关数据点，这些数据源自数据表的行或列。图表中的每个数据系列具有唯一的颜色或图案并且在图表的图例中表示。可以在图表中绘制一个或多个数据系列。饼图只有一个数据系列。）中各项的大小与各项总和的比例。饼图中的数据点 （数据点：在图表中绘制的单个值，这些值由条形、柱形、折线、饼图或圆环图的扇面、圆点和其他被称为数据标记的图形表示。相同颜色的数据标记组成一个数据系列。）显示为整个饼图的百分比。
#### （6）百分位数图
统计学术语，如果将一组数据从小到大排序，并计算相应的累计百分位，则某一百分位所对应数据的值就称为这一百分位的百分位数。可表示为：一组n个观测值按数值大小排列。如，处于p%位置的值称第p百分位数。
#### （7）散布图
散布图，是用来表示一组成对的数据之间是否有相关性的一种图表。这种成对的数据或许是“特性—要因”、“特性—特性”、“要因—要因”的关系。制作散布图的目的是为辨认一个品质特征和一个可能原因因素之间的联系。
散布图是用非数学的方式来辨认某现象的测量值与可能原因因素之间的关系。散布图又叫相关图，它是将两个可能相关的变数资料用点画在坐标图上，用成对的资料之间是否有相关性。
### 4.1.2 可视化时间空间数据
#### （1）等高线图
等高线地图就是将地表高度相同的点连成一环线直接投影到平面形成水平曲线，不同高度的环线不会相合，除非地表显示悬崖或峭壁才能使某处线条太密集出现重叠现像，若地表出线平坦开阔的山坡，曲线间之距离就相当宽，而它的基准线是以海平面的平均海潮位线为准，每张地图下方皆有制作标示说明，让使用者方便使用，主要图示有比例尺、图号、图幅接合表、图例与方位偏角度。
#### （2）曲面图
排列在工作表的列或行中的数据可以绘制到曲面图中。如果您要找到两组数据之间的最佳组合，可以使用曲面图。就像在地形图中一样，颜色和图案表示具有相同数值范围的区域。
#### （3）矢量场图
在某些数据中，一个特性可能同时具有值和方向。在这种情况下，同时显示方向和量的图可能是有用的。这种类型的图称作矢量图。
### 4.1.3 可视化高维数据
#### （1）矩阵
图像可以看作像素的矩阵阵列，其中每个像素用它的颜色和亮度刻画，数据矩阵是值的矩形阵列。
#### （2）平行坐标系
为了表示在高维空间的一个点集， 在N条平行的线的背景下，（一般这N条线都竖直且等距），一个在高维空间的点被表示为一条拐点在N条平行坐标轴的折线，在第K个坐标轴上的位置就表示这个点在第K个维的值。
平行坐标是信息可视化的一种重要技术。为了克服传统的笛卡尔直角坐标系容易耗尽空间、 难以表达三维以上数据的问题， 平行坐标将高维数据的各个变量用一系列相互平行的坐标轴表示， 变量值对应轴上位置。 为了反映变化趋势和各个变量间相互关系，往往将描述不同变量的各点连接成折线。所以平行坐标图的实质是将 维欧式空间的一个点Xi(xi1,xi2,...,xim) 映射到维平面上的一条曲线。
平行坐标图可以表示超高维数据。 平行坐标的一个显著优点是其具有良好的数学基础， 其射影几何解释和对偶特性使它很适合用于可视化数据分析。



参考文献：
[1] Pang-Ning Tan, Michael Steinbach, Vipin Kumar. 数据挖掘导论[M]. 人民邮电出版社, 2011.
[2] 周志华. 机器学习[M]. 清华大学出版社, 2016