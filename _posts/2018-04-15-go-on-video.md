---
layout:     post
title:      "继续研究pyqt"
subtitle:   "2018/04/15 学pyqt"
date:       2018-04-15
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20180415.jpg?raw=true"
tags:
    - 日记
    - pyqt
    - Qt界面编程
---

```
    今天，看了一天pyqt，太有趣啦！
```


## PyQt注意事项

### 使用eric6

`eric6`是一个非常好用的PyQt结合的框架，里面集成了`QtDesigner`。非常方便的将Qt编程和Python编程结合到一起。

### 编界面程序是需要注意的事项

#### 界面编程顺序

在编写界面程序时，需要首先在`Forms`标签里面`New Form`生成一个新的后缀为`.ui`的文件，此时自动出现`QtDesigner`出现`QtDesigner`
的界面拖拽框，我们可以在里面实现自己想要的功能。同时可以实现简单的界面内控件的信号和槽函数连接
的操作。完成后，关闭并保存。

如果需要高级的信号和槽操作，即，自己实现代码的槽函数。我们实现。


#### python打包

使用`pyinstaller`可以打包程序，直接在python里面，找打`main`函数的`.py`文件，输入命令

```
pyinstaller test.py  #打包成文件夹
pyinstaller -F test.py  #打包成单个文件 
pyinstaller -F -w test.py  #打包成窗口文件
pyinstaller -F -w -i 图标路径 test.py  #打包成窗口文件并制定图标
```