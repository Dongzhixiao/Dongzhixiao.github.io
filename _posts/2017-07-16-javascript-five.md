---
layout:     post
title:      "JavaScript学习（五）"
subtitle:   "2017/07/16 JavaScript"
date:       2017-07-16
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170716.jpg?raw=true"
tags:
    - JavaScript
---

### 时间:2017年4月28日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

#一、Table对象

rows属性

描述:返回表格的tr对象组成的集合

语法:collection object.rows

rowIndex

描述:获取行对象的索引数

语法:int object.rowIndex


insertRow()

描述:插入行

语法:rowElement object.insertRow(index)

说明:index为新行的索引值,其编号从0开始。

deleteRow()

描述:删除行

语法:object.deleteRow(index)



TR对象

cells属性

语法:返回行对象中单元格对象组成的集合

语法:collection object.cells

insertCell()

描述:在行对象内插入单元格

语法:cellElement object.insertCell(index)

deleteCell()

描述:删除列

语法:object.deleteCell(index)





#二、window对象

setTimeout()
描述:设置一次性定时器
语法:int window.setTimeout(string code,int time)

setInterval()
描述:设置周期性定时器
语法:int window.setInterval(string code,int time)

clearTimeout()

描述:清理由setTimeout()方法设置的定时器
语法:window.clearTimeout(int timeId)

clearInterval()

描述:清理由setInterval()方法设置的定时器

语法:window.clearInterval(int timeId)

open()
描述:打开浏览器窗口
语法:window.open(url)

alert()

描述:弹出警示对话框

语法:window.alert(string message)

confirm()

描述:弹出询问对话框

语法:boolean window.confirm(string message)

close()

描述:关闭窗口

语法:window.close()

说明:现在的某些浏览器,window.close()只能

关闭由window.open()方法打开的窗口。


#三、BOM

##1.什么是BOM?

BOM[Browser Object Model],浏览器对象模型

BOM提供与浏览器相关的API。

##2.screen对象

width属性

描述:返回显示器分辨率的宽度
语法:screen.width

height属性
描述:返回显示器分辨率的高度
语法:screen.height

##3.history

length属性
描述:返回历史的长度
语法:history.length

forward()
描述:前进
语法:history.forward()

back()
描述:后退
语法:history.back()

go()
描述:前进或后退
语法:history.go(int)
例:
history.go(3),前进3步
history.go(-2),后退两步

##4.location对象

href属性

描述:获取/设置浏览器URL地址

语法:location.href = value

reload()
描述:重新加载页面
语法:location.reload()

##5.navigator

appVersion
描述:返回浏览器的版本号
语法:navigator.appVersion

BOM整体方法图像可以如下图显示：
![BOM](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/BOM.png?raw=true "BOM整体方法图像")

#六、JS中的OOP

##1.直接量方式

var 变量名称 = {}

##2.原型方式

function 类名称([参数[,...]]){
   
}


#七、JSON

JSON是一种轻量级的数据交换格式。


JSON的官网:

http://www.json.org


