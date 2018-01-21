---
layout:     post
title:      "python学习（二）"
subtitle:   "2017/07/30 字符串常用方法及注释"
date:       2017-07-30
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年7月30日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

##字符串的方法及注释
<table cellspacing="0" class="t_table" style="width:80%"><tr><td width="206">capitalize()<br />
</td><td width="362">把字符串的第一个字符改为大写<br />
</td></tr><tr><td width="206">casefold()<br />
</td><td width="362">把整个字符串的所有字符改为小写<br />
</td></tr><tr><td width="206">center(width)<br />
</td><td width="362">将字符串居中，并使用空格填充至长度 width 的新字符串<br />
</td></tr><tr><td width="206">count(sub[, start[, end]])<br />
</td><td width="362">返回 sub 在字符串里边出现的次数，start 和 end 参数表示范围，可选。<br />
</td></tr><tr><td width="206">encode(encoding='utf-8', errors='strict')<br />
</td><td width="362">以 encoding 指定的编码格式对字符串进行编码。<br />
</td></tr><tr><td width="206">endswith(sub[, start[, end]])<br />
</td><td width="362">检查字符串是否以 sub 子字符串结束，如果是返回 True，否则返回 False。start 和 end 参数表示范围，可选。<br />
</td></tr><tr><td width="206">expandtabs([tabsize=8])<br />
</td><td width="362">把字符串中的 tab 符号（\t）转换为空格，如不指定参数，默认的空格数是 tabsize=8。<br />
</td></tr><tr><td width="206">find(sub[, start[, end]])<br />
</td><td width="362">检测 sub 是否包含在字符串中，如果有则返回索引值，否则返回 -1，start 和 end 参数表示范围，可选。<br />
</td></tr><tr><td width="206">index(sub[, start[, end]])<br />
</td><td width="362">跟 find 方法一样，不过如果 sub 不在 string 中会产生一个异常。<br />
</td></tr><tr><td width="206">isalnum()<br />
</td><td width="362">如果字符串至少有一个字符并且所有字符都是字母或数字则返回 True，否则返回 False。<br />
</td></tr><tr><td width="206">isalpha()<br />
</td><td width="362">如果字符串至少有一个字符并且所有字符都是字母则返回 True，否则返回 False。<br />
</td></tr><tr><td width="206">isdecimal()<br />
</td><td width="362">如果字符串只包含十进制数字则返回 True，否则返回 False。<br />
</td></tr><tr><td width="206">isdigit()<br />
</td><td width="362">如果字符串只包含数字则返回 True，否则返回 False。<br />
</td></tr><tr><td width="206">islower()<br />
</td><td width="362">如果字符串中至少包含一个区分大小写的字符，并且这些字符都是小写，则返回 True，否则返回 False。<br />
</td></tr><tr><td width="206">isnumeric()<br />
</td><td width="362">如果字符串中只包含数字字符，则返回 True，否则返回 False。<br />
</td></tr><tr><td width="206">isspace()<br />
</td><td width="362">如果字符串中只包含空格，则返回 True，否则返回 False。<br />
</td></tr><tr><td width="206">istitle()<br />
</td><td width="362">如果字符串是标题化（所有的单词都是以大写开始，其余字母均小写），则返回 True，否则返回 False。<br />
</td></tr><tr><td width="206">isupper()<br />
</td><td width="362">如果字符串中至少包含一个区分大小写的字符，并且这些字符都是大写，则返回 True，否则返回 False。<br />
</td></tr><tr><td width="206">join(sub)<br />
</td><td width="362">以字符串作为分隔符，插入到 sub 中所有的字符之间。<br />
</td></tr><tr><td width="206">ljust(width)<br />
</td><td width="362">返回一个左对齐的字符串，并使用空格填充至长度为 width 的新字符串。<br />
</td></tr><tr><td width="206">lower()<br />
</td><td width="362">转换字符串中所有大写字符为小写。<br />
</td></tr><tr><td width="206">lstrip()<br />
</td><td width="362">去掉字符串左边的所有空格<br />
</td></tr><tr><td width="206">partition(sub)<br />
</td><td width="362">找到子字符串 sub，把字符串分成一个 3 元组 (pre_sub, sub, fol_sub)，如果字符串中不包含 sub 则返回 ('原字符串', '', '')<br />
</td></tr><tr><td width="206">replace(old, new[, count])<br />
</td><td width="362">把字符串中的 old 子字符串替换成 new 子字符串，如果 count 指定，则替换不超过 count 次。<br />
</td></tr><tr><td width="206">rfind(sub[, start[, end]])<br />
</td><td width="362">类似于 find() 方法，不过是从右边开始查找。<br />
</td></tr><tr><td width="206">rindex(sub[, start[, end]])<br />
</td><td width="362">类似于 index() 方法，不过是从右边开始。<br />
</td></tr><tr><td width="206">rjust(width)<br />
</td><td width="362">返回一个右对齐的字符串，并使用空格填充至长度为 width 的新字符串。<br />
</td></tr><tr><td width="206">rpartition(sub)<br />
</td><td width="362">类似于 partition() 方法，不过是从右边开始查找。<br />
</td></tr><tr><td width="206">rstrip()<br />
</td><td width="362">删除字符串末尾的空格。<br />
</td></tr><tr><td width="206">split(sep=None, maxsplit=-1)<br />
</td><td width="362">不带参数默认是以空格为分隔符切片字符串，如果 maxsplit 参数有设置，则仅分隔 maxsplit 个子字符串，返回切片后的子字符串拼接的列表。<br />
</td></tr><tr><td width="206">splitlines(([keepends]))<br />
</td><td width="362">按照 '\n' 分隔，返回一个包含各行作为元素的列表，如果 keepends 参数指定，则返回前 keepends 行。<br />
</td></tr><tr><td width="206">startswith(prefix[, start[, end]])<br />
</td><td width="362">检查字符串是否以 prefix 开头，是则返回 True，否则返回 False。start 和 end 参数可以指定范围检查，可选。<br />
</td></tr><tr><td width="206">strip([chars])<br />
</td><td width="362">删除字符串前边和后边所有的空格，chars 参数可以定制删除的字符，可选。<br />
</td></tr><tr><td width="206">swapcase()<br />
</td><td width="362">翻转字符串中的大小写。<br />
</td></tr><tr><td width="206">title()<br />
</td><td width="362">返回标题化（所有的单词都是以大写开始，其余字母均小写）的字符串。<br />
</td></tr><tr><td width="206">translate(table)<br />
</td><td width="362">根据 table 的规则（可以由 str.maketrans('a', 'b') 定制）转换字符串中的字符。<br />
</td></tr><tr><td width="206">upper()<br />
</td><td width="362">转换字符串中的所有小写字符为大写。<br />
</td></tr><tr><td width="206">zfill(width)<br />
</td><td width="362">返回长度为 width 的字符串，原字符串右对齐，前边用 0 填充。<br />
</td></tr></table>

##字符串格式化符号含义
<table cellspacing="0" class="t_table" style="width:500px"><tr><td><div align="center"><strong>符号</strong></div></td><td><div align="center"><strong>说明</strong></div></td></tr><tr><td><div align="center">%c</div></td><td> 格式化字符及其 ASCII 码</td></tr><tr><td><div align="center">%s</div></td><td> 格式化字符串</td></tr><tr><td><div align="center">%d</div></td><td> 格式化整数</td></tr><tr><td><div align="center">%o</div></td><td> 格式化无符号八进制数</td></tr><tr><td><div align="center">%x</div></td><td> 格式化无符号十六进制数</td></tr><tr><td><div align="center">%X</div></td><td> 格式化无符号十六进制数（大写）</td></tr><tr><td><div align="center">%f</div></td><td> 格式化浮点数字，可指定小数点后的精度</td></tr><tr><td><div align="center">%e</div></td><td> 用科学计数法格式化浮点数</td></tr><tr><td><div align="center">%E</div></td><td> 作用同 %e，用科学计数法格式化浮点数</td></tr><tr><td><div align="center">%g</div></td><td> 根据值的大小决定使用 %f 或 %e</td></tr><tr><td><div align="center">%G</div></td><td> 作用同 %g，根据值的大小决定使用 %f 或者 %E</td></tr></table><br />

##格式化操作符辅助命令

<table cellspacing="0" class="t_table" style="width:500px"><tr><td><div align="center"><strong>符号</strong></div></td><td><div align="center"><strong>说明</strong></div></td></tr><tr><td><div align="center">m.n</div></td><td> m 是显示的最小总宽度，n 是小数点后的位数</td></tr><tr><td><div align="center">-</div></td><td> 用于左对齐</td></tr><tr><td><div align="center">+</div></td><td> 在正数前面显示加号（+）</td></tr><tr><td><div align="center">#</div></td><td> 在八进制数前面显示 '0o'，在十六进制数前面显示 '0x' 或 '0X'</td></tr><tr><td><div align="center">0</div></td><td> 显示的数字前面填充 '0' 取代空格</td></tr></table><br />

##Python 的转义字符及其含义

<table cellspacing="0" class="t_table" style="width:500px"><tr><td><div align="center"><strong>符号</strong></div></td><td><div align="center"><strong>说明</strong></div></td></tr><tr><td><div align="center">\'</div></td><td> 单引号</td></tr><tr><td><div align="center">\&quot;</div></td><td> 双引号</td></tr><tr><td><div align="center">\a</div></td><td> 发出系统响铃声</td></tr><tr><td><div align="center">\b</div></td><td> 退格符</td></tr><tr><td><div align="center">\n</div></td><td> 换行符</td></tr><tr><td><div align="center">\t</div></td><td> 横向制表符（TAB）</td></tr><tr><td><div align="center">\v</div></td><td> 纵向制表符</td></tr><tr><td><div align="center">\r</div></td><td> 回车符</td></tr><tr><td><div align="center">\f</div></td><td> 换页符</td></tr><tr><td><div align="center">\o</div></td><td> 八进制数代表的字符</td></tr><tr><td><div align="center">\x</div></td><td> 十六进制数代表的字符</td></tr><tr><td><div align="center">\0</div></td><td> 表示一个空字符</td></tr><tr><td><div align="center">\\</div></td><td> 反斜杠</td></tr></table><br />
<br />
</td></tr></table>