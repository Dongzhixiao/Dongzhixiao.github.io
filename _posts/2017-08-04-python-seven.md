---
layout:     post
title:      "python学习（七）"
subtitle:   "2017/08/04 time模块相关函数"
date:       2017-08-04
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年8月4日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

##time 模块 -- 时间获取和转换


time 模块提供各种时间相关的功能<br />
在 Python 中，与时间处理有关的模块包括：time，datetime 以及 calendar<br />

###必要说明：

- 虽然这个模块总是可用，但并非所有的功能都适用于各个平台。
- 该模块中定义的大部分函数是调用 C 平台上的同名函数实现，所以各个平台上实现可能略有不同。

###一些术语和约定的解释：

<ul><li>时间戳（timestamp）的方式：通常来说，时间戳表示的是从 1970 年 1 月 1 日 00:00:00 开始按秒计算的偏移量（time.gmtime(0)）此模块中的函数无法处理 1970 纪元年以前的日期和时间或太遥远的未来（处理极限取决于 C 函数库，对于 32 位系统来说，是 2038 年）<li>UTC（Coordinated Universal Time，世界协调时）也叫格林威治天文时间，是世界标准时间。在中国为 UTC+8<li>DST（Daylight Saving Time）即夏令时的意思<li>一些实时函数的计算精度可能低于它们建议的值或参数，例如在大部分 Unix 系统，时钟一秒钟“滴答”50~100 次<br />
</ul><br />
<br />

###时间元祖（time.struct_time）：

gmtime()，localtime() 和 strptime() 以时间元祖（struct_time）的形式返回。<br />

<table cellspacing="0" class="t_table" style="width:98%"><tr><td><strong> 索引值（Index）</strong></td><td><strong>属性（Attribute） </strong></td><td><strong> 值（Values）</strong></td></tr><tr><td> 0</td><td> tm_year（年）</td><td> （例如：2015）</td></tr><tr><td> 1</td><td> tm_mon（月）</td><td> 1 ~ 12</td></tr><tr><td> 2</td><td> tm_mday（日）</td><td> 1 ~ 31</td></tr><tr><td> 3</td><td> tm_hour（时）</td><td> 0 ~ 23</td></tr><tr><td> 4</td><td> tm_min（分）</td><td> 0 ~ 59</td></tr><tr><td> 5</td><td> tm_sec（秒）</td><td> 0 ~ 61（见下方注1）</td></tr><tr><td> 6</td><td> tm_wday（星期几）</td><td> 0 ~ 6（0 表示星期一）</td></tr><tr><td> 7</td><td> tm_yday（一年中的第几天）</td><td> 1 ~ 366</td></tr><tr><td> 8</td><td> tm_isdst（是否为夏令时）</td><td> 0， 1， -1（-1 代表夏令时）</td></tr></table><br />
<i>注1：范围真的是 0 ~ 61（你没有看错哦^_^）；60 代表闰秒，61 是基于历史原因保留。</i><br />

<strong>time.altzone</strong><br />
<br />
返回格林威治西部的夏令时地区的偏移秒数；如果该地区在格林威治东部会返回负值（如西欧，包括英国）；对夏令时启用地区才能使用。<br />
<br />
<br />
<strong>time.asctime([t])</strong><br />
<br />
接受时间元组并返回一个可读的形式为&quot;Tue Dec 11 18:07:14 2015&quot;（2015年12月11日 周二 18时07分14秒）的 24 个字符的字符串。<br />
<br />
<br />
<strong>time.clock()</strong><br />
<br />
用以浮点数计算的秒数返回当前的 CPU 时间。用来衡量不同程序的耗时，比 time.time() 更有用。<br />
<br />
<i>Python 3.3 以后不被推荐，由于该方法依赖操作系统，建议使用 perf_counter() 或 process_time() 代替（一个返回系统运行时间，一个返回进程运行时间，请按照实际需求选择）<br />
</i><br />
<br />
<strong>time.ctime([secs]) </strong><br />
<br />
作用相当于 asctime(localtime(secs))，未给参数相当于 asctime()<br />
<br />
<br />
<strong>time.gmtime([secs])</strong><br />
<br />
接收时间辍（1970 纪元年后经过的浮点秒数）并返回格林威治天文时间下的时间元组 t（注：t.tm_isdst 始终为 0）<br />
<br />
<br />
<strong>time.daylight</strong><br />
<br />
如果夏令时被定义，则该值为非零。<br />
<br />
<br />
<strong>time.localtime([secs])</strong><br />
<br />
接收时间辍（1970 纪元年后经过的浮点秒数）并返回当地时间下的时间元组 t（t.tm_isdst 可取 0 或 1，取决于当地当时是不是夏令时）<br />
<br />
<br />
<strong>time.mktime(t)</strong><br />
<br />
接受时间元组并返回时间辍（1970纪元后经过的浮点秒数）<br />
<br />
<br />
<strong>time.perf_counter()</strong><br />
<br />
返回计时器的精准时间（系统的运行时间），包含整个系统的睡眠时间。由于返回值的基准点是未定义的，所以，只有连续调用的结果之间的差才是有效的。<br />
<br />
<br />
<strong>time.process_time() </strong><br />
<br />
返回当前进程执行 CPU 的时间总和，不包含睡眠时间。由于返回值的基准点是未定义的，所以，只有连续调用的结果之间的差才是有效的。<br />
<br />
<br />
<strong>time.sleep(secs)</strong><br />
<br />
推迟调用线程的运行，secs 的单位是秒。<br />
<br />
<br />
<strong>time.strftime(format[, t]) </strong><br />
<br />
把一个代表时间的元组或者 struct_time（如由 time.localtime() 和 time.gmtime() 返回）转化为格式化的时间字符串。如果 t 未指定，将传入 time.localtime()。如果元组中任何一个元素越界，将会抛出 ValueError 异常。<br />
<br />
format 格式如下：<br />
<br />
<table cellspacing="0" class="t_table" style="width:98%"><tr><td> <strong>格式</strong></td><td> <strong>含义</strong></td><td> <strong>备注</strong></td></tr><tr><td> %a</td><td> 本地（locale）简化星期名称</td><td> </td></tr><tr><td> %A</td><td> 本地完整星期名称</td><td> </td></tr><tr><td> %b</td><td> 本地简化月份名称</td><td> </td></tr><tr><td> %B</td><td> 本地完整月份名称</td><td> </td></tr><tr><td> %c</td><td> 本地相应的日期和时间表示</td><td> </td></tr><tr><td> %d</td><td> 一个月中的第几天（01 - 31）</td><td> </td></tr><tr><td> %H</td><td> 一天中的第几个小时（24 小时制，00 - 23）</td><td> </td></tr><tr><td> %l</td><td> 一天中的第几个小时（12 小时制，01 - 12）</td><td> </td></tr><tr><td> %j</td><td> 一年中的第几天（001 - 366）</td><td> </td></tr><tr><td> %m</td><td> 月份（01 - 12）</td><td> </td></tr><tr><td> %M</td><td> 分钟数（00 - 59）</td><td> </td></tr><tr><td> %p</td><td> 本地 am 或者 pm 的相应符</td><td> <i>注1</i></td></tr><tr><td> %S</td><td> 秒（01 - 61）</td><td> <i>注2</i></td></tr><tr><td> %U</td><td> 一年中的星期数（00 - 53 星期天是一个星期的开始）第一个星期天之前的所有天数都放在第 0 周</td><td> <i>注3</i></td></tr><tr><td> %w</td><td> 一个星期中的第几天（0 - 6，0 是星期天）</td><td> <i>注3</i></td></tr><tr><td> %W</td><td> 和 %U 基本相同，不同的是 %W 以星期一为一个星期的开始</td><td> </td></tr><tr><td> %x</td><td> 本地相应日期</td><td> </td></tr><tr><td> %X</td><td> 本地相应时间</td><td> </td></tr><tr><td> %y</td><td> 去掉世纪的年份（00 - 99）</td><td> </td></tr><tr><td> %Y</td><td> 完整的年份</td><td> </td></tr><tr><td> %z</td><td> 用 +HHMM 或 -HHMM 表示距离格林威治的时区偏移（H 代表十进制的小时数，M 代表十进制的分钟数）</td><td> </td></tr><tr><td> %Z</td><td> 时区的名字（如果不存在为空字符）</td><td> </td></tr><tr><td> %%</td><td> %号本身</td><td> </td></tr></table><br />
<i>注1：“%p”只有与“%I”配合使用才有效果。</i><br />
<i>注2：范围真的是 0 ~ 61（你没有看错哦^_^）；60 代表闰秒，61 是基于历史原因保留。</i><br />
<i>注3：当使用 strptime() 函数时，只有当在这年中的周数和天数被确定的时候 %U 和 %W 才会被计算。</i><br />
<br />
举个例子：<br />
<div class="blockcode"><div id="code_F4v"><ol><li># I love FishC.com!<br />
<li>&gt;&gt;&gt; import time as t<br />
<li>&gt;&gt;&gt; t.strftime(&quot;a, %d %b %Y %H:%M:%S +0000&quot;, t.gmtime())<br />
<li>'a, 24 Aug 2014 14:15:03 +0000'<br />
<li></ol></div><em onclick="copycode($('code_F4v'));">复制代码</em></div><br />

<br />
<strong>time.strptime(string[, format])</strong><br />
<br />
把一个格式化时间字符串转化为 struct_time。实际上它和 strftime() 是逆操作。<br />
<br />
举个例子：<br />
<div class="blockcode"><div id="code_i2i"><ol><li># I really love DongZhiXiao!<br />
<li>&gt;&gt;&gt; import time as t<br />
<li>&gt;&gt;&gt; t.strptime(&quot;30 Nov 14&quot;, &quot;%d %b %y&quot;)<br />
<li>time.struct_time(tm_year=2014, tm_mon=11, tm_mday=30, tm_hour=0, tm_min=0, tm_sec=0, tm_wday=6, tm_yday=334, tm_isdst=-1)</ol>

<br />
<strong>time.time()</strong><br />
<br />
返回当前时间的时间戳（1970 纪元年后经过的浮点秒数）<br />
<br />
<br />
<strong>time.timezone</strong><br />
<br />
time.timezone 属性是当地时区（未启动夏令时）距离格林威治的偏移秒数（美洲 &gt;0；大部分欧洲，亚洲，非洲 &lt;= 0）<br />
<br />
<br />
<strong>time.tzname</strong><br />
<br />
time.tzname 属性是包含两个字符串的元组：第一是当地非夏令时区的名称，第二个是当地的 DST 时区的名称。<br />
<br />