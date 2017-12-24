---
layout:     post
title:      "新工作"
subtitle:   "心情平淡的一天"
date:       2017-04-28
author:     "WangXiaoDong"
header-img: "img/clannad.jpg"
tags:
    - 日记
    - Qt
    - 编译器
---


### 时间:2016年7月23日 天气:晴
-----
#####   Author:冬之晓:stuck_out_tongue_winking_eye:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------
<pre>
    今天，已经是来余姚的第三天了，这两天大概感受到了这边的居住环境，总体来说不
错，就是吃饭的地方比较少，这些到是无关紧要的。主要我一定要提高自己的水平，争取以
后能在程序的学习上面独当一面。  
    今天周六，在宿舍研究了一下程序为啥在我的电脑上没法运行。经过一天的尝试，终
于发现了问题所在。原来我的电脑里面为了兼容我以前的程序，下载了好多Qt版本，因此
里面也包含了好多不同的编译器，其中有StrawBerry、Msvc、MinGW！今天我需要将一个Q
t的第三方库导入到Qt里面，每次都不成功。
    最后成功了，原来我的系统Path里面有一个编译器的路径，所以每次我用其他版本Qt
进行Makefile文件生成，但是编译的时候默认用的是Path里面的编译器，不是对应Qt版本
里面带的，因此才会每次都不成功！！而且我也发现了Qt版本几个重要的文件存放，就拿
最新版本的Qt5.7.0来说，我下载的是MinGW版本，安装时Tools里面也安装了对应的编译 
器：
</pre>
>Qt5.7.0
>>**5.7**
>>>**mingw53_32**
>>>>**bin**
>>>>>**qmake.exe(将pro文件转换为makefile文件)**

>>dist  
Docs(Qt的说明文档)  
Examples(Qt的示例程序)  
Licenses  
**Tools**
>>>**mingw530_32**
>>>>**bin**
>>>>>**mingw32-make.exe(执行makefile文件来编译所以程序文件)**

>>vcredist  
<pre>
    如上所示，在加粗的路径上的qmake和make是在编译第三方库时比较重要的文件，同
时注意要把系统环境变量里面其他编译器的路径删除，只保留需要编译的qmake版本和mak
e的路径，这样才行！！
</pre>