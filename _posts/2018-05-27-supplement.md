---
layout:     post
title:      "周末休息"
subtitle:   "2018/05/27 休息"
date:       2018-05-27
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180527.jpg?raw=true"
tags:
    - 日记
---


```
    今天是周末，因此决定好好休息一下，早晨到实验室整理最近的日志，看到卢老师过来加班了，卢老师真的是
太辛苦啦！
```

今天，准备总结一下Qt调用子程序的方法，比如SPMF库。

### SPMF库

非常好用的关于序列分析和关联分析的库，该库实现了很多相关的算法，补全了weka库的不足。
最重要的是，这个库的接口非常清晰易懂，具有很好的命令行接口。里面的jar包通过java命令
即可直接运行！

### 使用subprocess直接调用即可

首先确定你的电脑中有`java`命令。然后从SPMF官网下载相关`jar包`。
然后使用编程语言对应的相关子程序调用的库即可，比如`python`语言就可以使用`subprocess`包进行调用使用

```
import subprocess
##使用命令行进行操作
cp=subprocess.run(['./jre/bin/java','-jar','spmf.jar','run','FPGrowth_itemsets','./test_files/contextPasquier99.txt','output.txt','40%'],
                      stdout=subprocess.PIPE,stderr=subprocess.STDOUT)
print(cp.stdout.decode('gbk'))
```