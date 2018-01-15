---
layout:     post
title:      "今年家乡的第一场雪"
subtitle:   "2016/11/22 在家休息"
date:       2016-11-22
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20161122.jpg?raw=true"
tags:
    - 日记
    - Qt
---

### 时间:2016年11月22日 天气:大雪:snowflake:
-----
#####   Author:冬之晓:relieved:
#####   Email: 347916416@qq.com
----------

<pre>
    今天又一下子睡到了中午，起床就发现洛阳突然下起了漂泊大雪！！积雪非常的厚，令
我非常震惊。下午，我和妈妈冒着积雪来到了姥姥家，我顺便换了一个小卡，然后把小卡放
到新买的小米5手机里面，感觉不错！晚上回家的时候，让爸爸带着我们走了一路，但是路
上非常的滑！不过还是顺利回家了，希望明天不要再下雪了，真的是非常不方便！
</pre>

### QMessageBox的常用方法用总结

#### 普通用法

QMessageBox是在编程中非常常用的一个类，特别是我们需要要几个小小的提示而又不需要太多的逻辑的对话框。这时，最简单的用法就是直接使用QMessageBox的静态方法生成对话框，对于一般只需要简单提示的情况下这样就够用了，举个例子：

```C++
QMessageBox::information(this, tr("提示信息"), tr("与控制器异常连接"), QMessageBox::NoButton);    
```

这样就是一个简单的使用。


#### 自定义按钮用法

普通用法出现的对话框的缺点是按钮上面显示的子都是固定的英文，如果需要自定义按钮上面显示的内容，需要下面的方式：

```C++
QString DoTip("是否确定将");
DoTip = DoTip.append(DO_label.at(num)->text()).append("的值由").
                  append(DO_values.at(num)->text()).append("设置为").append(QString::number(tipnum));
QMessageBox temMB(QMessageBox::Warning,tr("警告"),DoTip,QMessageBox::Yes | QMessageBox::No);   //!< @bug 后期可以多项提示
temMB.setButtonText(QMessageBox::Yes,tr("是"));
temMB.setButtonText(QMessageBox::No,tr("否"));
if(temMB.exec() != QMessageBox::Yes)
    return;
```

上面就是一个例子，使得出现的对话框显示的是中文。