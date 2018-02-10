---
layout:     post
title:      "Qt程序打包"
subtitle:   "2016/08/18 Qt程序安装包处理"
date:       2016-08-18
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160818.jpg?raw=true"
tags:
    - 日记
    - Qt
---

### 时间:2016年8月18日 天气:晴:sunny:
-----
#####   Author:冬之晓:dizzy_face:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天，我将合并后的代码发给项目经理进行测试，但是他问我为什么程序不是一个安
装包？我一想，也是啊，我们平时在电脑上安装一个程序本身下载下来就是一个安装包，
如果下载下来是一堆文件的话肯定没人会用！一般编写手机端应用程序的话自动能够生成
一个安装包，但是使用Qt进行PC端编程的时候必须自己做成一个安装包的。我以前都是
编写完程序，能够运行就行了，所以忽略了这一块，今天决定仔细研究一下！
</pre>

#### 编写完的程序进行安装包的生成

为了研究将程序进行打包，首先必须保证自己的文件已经完整了，能够在任何一台电脑上运行。
下面一点一点介绍。由于我使用的是Windows操作系统，所有下面的配置都仅仅代表Windows平台。

##### 生成Qt所有的依赖文件

当你将程序成功运行之后，会在release文件夹下面生成一个`.exe`后缀的可执行文件，这个文件一般不能直接打开，它会依赖很多Qt自己的动态链接库，如果自己在一台普通电脑上，尝试打开，就会提示缺少某些`.dll`文件。这时我们可以在Qt的安装目录下面寻找这些`.dll`文件，以我的Qt5.7版本为例子，其目录一般是

`你自己安装Qt的路径:\Qt\Qt5.7.0\5.7\mingw53_32\bin`

当然，这种方法比较慢，因为一般一个中型的程序可能需要依赖很多`.dll`文件，有一个简单方法，在上面这个目录下面，Qt会提供一个名叫`windeployqt.exe`的程序。这个程序是专门为Qt程序自动寻找需要发布的程序所需要依赖项。首先你需要将release版本下生成的可执行文件放到一个文件夹内，比如我的一个程序名叫`test.ext`，放置在路径`C:\Users\Administrator\Desktop\test`下，如图：![截图](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/testBefore.png "使用windeployqt前")


然后在当前文件夹下打开命令行输入(我已经把`你自己安装Qt的路径:\Qt\Qt5.7.0\5.7\mingw53_32\bin`加入到系统PATH变量里面了，因此可以直接使用)：

```bat
cd C:\Users\Administrator\Desktop\test
windeployqt test.exe
```

这样运行之后就发现该路径下多了很多Qt的动态链接库，如图：![截图](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/testAfter.png "使用windeployqt后")

这样，在一般电脑上就可以运行了。当然有时还是无法运行，这种情况下可能你使用了第三方库。但是这样通常情况下需要的文件比较少，因此可以根据提示缺少的`.dll`文件单独进行拷贝，减少了很多步骤。

##### 使用安装工具

在得到可以在普通机器上运行的所有程序和其依赖文件后，就可以进行打包。通过网上调研，我发现了很多简单的打包工具。当然这都不符合我的胃口，我喜欢比较有意思的打包工具，最后就决定使用`NSIS`软件。NSIS（Nullsoft Scriptable Install System）是一个开源的 Windows 系统下安装程序制作程序。它提供了安装、卸载、系统设置、文件解压缩等功能。这如其名字所指出的那样，NSIS 是通过它的脚本语言来描述安装程序的行为和逻辑的。虽然比较复杂，但是这样就可以自己定制安装程序的每一个步骤。
首先需要到[下载页面](http://nsis.sourceforge.net/Download "NSIS下载页面")进行下载。我找到了一个比较全的中文安装包，里面有好的相关帮助资料和脚本编辑器，在[这里](https://github.com/Dongzhixiao/PictureCache/blob/master/software/NSIS_发布软件工具.rar "中文安装包")可以下载。
安装后可以打开“VNISEdit 编译环境”，然后在“文件->新建脚本->向导”里面一步一步选择，就能进行简单的安装包生成。当然复杂的方式如何使用呢？等我下次有时间继续研究后再补充。