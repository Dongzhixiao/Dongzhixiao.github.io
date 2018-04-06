---
layout:     post
title:      "去中国国家博物馆"
subtitle:   "2018/04/01 游玩"
date:       2018-04-01
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180401.jpg?raw=true"
tags:
    - 日记
---

```
    今天是周日，和同学杭杰约好了一起去中国国家博物馆参观。早晨六点多我就起床了，因为昨天和杭杰约好，
他7点回来我们学校吃饭。我7点10分给杭杰发微信，但是他迟迟没有回应。最后才知道，原来杭杰最近身体不舒服，所以
就起床晚了。然后为了节约时间，我就到他那里吃饭了。
    吃完饭，我们一起徒步去了博物馆，走了整整一上午。虽然花费了很长的时间，但是在路上我和他谈论着相关
研究的领域和最近读书的一些体会，感觉非常开心。我非常喜欢和别人讨论学习的问题，关键是有人愿意倾听你
讲的内容，还愿意和你分享他学习的知识，这样的时光是最让人快乐的！
    不知不觉，我就就走到了天安门广场，第一次来天安门。在祖国首都的正中心，内心充满着对祖国伟大富强的
骄傲和自豪。感到身为中国人，一定要努力学习，为祖国的发展尽一份自己的力量！
    下午，回来后，我们愉快的吃了一顿鱼，就各种会宿舍了。到宿舍后才感觉全身都动不了了。哎，自己的体质
真差，以后一定要加强锻炼啦！
```

今天拍的照片如下：

![照片](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180401_1.jpg?raw=true)
![照片](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180401_2.jpg?raw=true)
![照片](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180401_3.jpg?raw=true)
![照片](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180401_4.jpg?raw=true)
![照片](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180401_5.jpg?raw=true)


今天总结一下上周使用的正态分布测试方法

## 正态分布测试

上周在学习实践的过程中，我需要测试一组数据是否属于正态分布，于是就搜索的相关函数，发现`scipy`模块里面有相关的测试函数。
具体在`scipy.stats`包里面。这个包和上面介绍的`Statsmodels`类似，都输入统计学相关的包，只是上面的包分析的内容更多，而
`scipy.stats`包则突出一些基础函数的分布律和概率密度函数。

比如整体分布测，我们就可用`scipy.stats.normaltest`函数，例子如下：

```
from scipy import stats
pts = 1000
np.random.seed(28041990)
a = np.random.normal(0, 1, size=pts)
b = np.random.normal(2, 1, size=pts)
x = np.concatenate((a, b))
k2, p = stats.normaltest(x)
alpha = 1e-3
print("p = {:g}".format(p))
if p < alpha:  # null hypothesis: x comes from a normal distribution
    print("The null hypothesis can be rejected")
else:
print("The null hypothesis cannot be rejected")
```

我还根据读入的`dict`数据和传入的标签序号或者角标，绘制一个梯形图个对于正态分布的概率密度的函数，如下：

> 注意：dict数据键对应要绘制的数据名称，值对应数据列表

```
def DrawHistogram(dd, num):   #输入索引序号或者角标
    keysList = num if type(num) == str else list(dd.keys())[num]
    valuesList = dd[num] if type(num) == str else list(dd.values())[num]
    
#    print(keysList,valuesList)
    
    arrForCalculate = np.array(valuesList)
    mu = arrForCalculate.mean()  # mean of distribution
    sigma = arrForCalculate.std()  # standard deviation of distribution
   
    num_bins = 50
    
    fig, ax = plt.subplots()

    # the histogram of the data
    n, bins, patches = ax.hist(valuesList, num_bins, density=1)
    
    # add a 'best fit' line
    y = ((1 / (np.sqrt(2 * np.pi) * sigma)) *
     np.exp(-0.5 * (1 / sigma * (bins - mu))**2))
    ax.plot(bins, y, '--')
    ax.set_xlabel('log numbers in per time segment')
    ax.set_ylabel('Probability density')
    ax.set_title('Histogram of %s: $\mu=%.2f$, $\sigma=%.2f$' % (keysList,mu,sigma))
    
    # Tweak spacing to prevent clipping of ylabel
    fig.tight_layout()
    plt.show()
```
