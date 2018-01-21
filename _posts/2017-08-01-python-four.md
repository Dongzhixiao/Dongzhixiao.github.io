---
layout:     post
title:      "python学习（四）"
subtitle:   "2017/08/01 文件相关方法"
date:       2017-08-01
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年8月1日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

## 文件打开模式
<table cellspacing="0" class="t_table" style="width:500px"><tr><td><strong> 打开模式</strong></td><td><strong> 执行操作</strong></td></tr><tr><td> 'r'</td><td> 以只读方式打开文件（默认）</td></tr><tr><td> 'w'</td><td> 以写入的方式打开文件，会覆盖已存在的文件</td></tr><tr><td> 'x'</td><td> 如果文件已经存在，使用此模式打开将引发异常</td></tr><tr><td> 'a'</td><td> 以写入模式打开，如果文件存在，则在末尾追加写入</td></tr><tr><td> 'b'</td><td> 以二进制模式打开文件</td></tr><tr><td> 't'</td><td> 以文本模式打开（默认）</td></tr><tr><td> '+'</td><td> 可读写模式（可添加到其他模式中使用）</td></tr><tr><td> 'U'</td><td> 通用换行符支持</td></tr></table><br />

## 文件对象方法
<table cellspacing="0" class="t_table" style="width:500px"><tr><td><strong> 文件对象方法</strong></td><td><strong> 执行操作</strong></td></tr><tr><td> f.close()</td><td> 关闭文件</td></tr><tr><td> f.read([size=-1])</td><td> 从文件读取size个字符，当未给定size或给定负值的时候，读取剩余的所有字符，然后作为字符串返回</td></tr><tr><td> f.readline([size=-1])</td><td>从文件中读取并返回一行（包括行结束符），如果有size有定义则返回size个字符</td></tr><tr><td> f.write(str)</td><td> 将字符串str写入文件</td></tr><tr><td> f.writelines(seq)</td><td> 向文件写入字符串序列seq，seq应该是一个返回字符串的可迭代对象<br />
