---
layout:     post
title:      "适应生活"
subtitle:   "2016/07/27 慢慢开始适应这边的生活"
date:       2016-07-27
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20160727.jpg?raw=true"
tags:
    - 日记
    - Windows命令
---

### 时间:2016年7月27日 天气:晴转雷阵雨:sunny:--->:zap:
-----
#####   Author:冬之晓:sleepy:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    早上我和江威一起去上班，我们住的地方离公司并不远，到了公司
江威已经满头大汗，他说这边天气真的是太热了。没错，余姚确实比西安在这个季节热一点，
这边的天气特点就是湿热，因为周围有很多河流。
    经过一天的工作，江威说他感觉在这边待不了太长时间，因为这边的季节实在是太不
适宜了。其实对我来说倒没有什么，因为我本身适应力就比较强，感觉到哪都差不多，
而且我平常也不怎么闲逛，有一个住的地方再吃的方便就感觉很好了……
    临近下午快下班的时候，下了一阵雨，下班的时候天气比往常凉快一点，真希望
每天都能这样。下班后，江威又找到我，让我带他逛一下超市。我们就去超市转了一大圈
，晚上回来就很累了，决定今天就早点休息吧。
</pre>

今天比较累了，就少看点东西，只聊聊几个简单的命令吧~
##### 检查系统信息
通常在使用用户计算机时，可能需要检查一些基本的系统信息。这时常用的命令有下面四个：
- [X] date。可以显示并设置当前系统的日期。
- [X] time。可以显示并设置当前系统的时间。
- [X] whoami。显示当前登录到系统的用户名。
- [X] where。使用某种搜索模式搜索文件，并返回匹配结果。

前面三个命令自己尝试一下就好，非常简单。关键是第四个命令需要解释一下：  
默认情况下，`where`命令会在当前目录以及环境变量`path`指定的路径中进行搜索。因此，只要在命令shell窗口中输入`where`命令，其后跟随要搜索的可执行程序名，就可以快速地在当前路径中搜索该可执行程序。比如输入：  
`where cmd.exe`  
上面命令的输出结果为`cmd.exe`的全文件路径：  
`C:\Windows\System32\cmd.exe`  
使用`where`命令还有一种常用方法是：  
`where /r baseDir filename`
`/r`代表从指定目录(baseDir)开始递归搜索，包含所有子目录。filename代表要搜索文件的全名或者部分名字，其中可以包括通配符。`?`匹配单个字符，`*`匹配多个字符。比如，data???.txt或者data\*.\*。比如下面命令：  
- `where /r C:\data*.txt`  
代表在C:\目录及其子目录下搜索文件名以data开始的文本文件。
- `where /r C:\data*.*`  
代表搜索文件名以data开始的所有类型文件。

