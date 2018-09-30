---
layout:     post
title:      "修改代码"
subtitle:   "2018/09/25 修改"
date:       2018-09-25
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180925.jpg?raw=true"
tags:
    - 日记
---


```
    今天主要完成的代码工作是如何通过读取所有模型来从新计算精度。我在测试的时候发现读取模型的时候越往后
读取速度越慢，最后找到原因。原来是模型在读取的时候，每次都是会把计算图全部存下来，因此每次读取模型后都
要在下次读取之前都要使用K.clear_session()这个后端函数将所有计算图都清空，这样在下次之前就会发现完全不
卡了！
```



