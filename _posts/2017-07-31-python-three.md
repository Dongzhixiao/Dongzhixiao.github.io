---
layout:     post
title:      "python学习（三）"
subtitle:   "2017/07/31 集合类型内建方法总结"
date:       2017-07-31
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年7月31日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

## 集合类型内建方法总结
<table cellspacing="0" class="t_table" style="width:98%"><tr><td><div align="center"><strong> 集合（s）.方法名</strong></div></td><td><div align="center"><strong>等价符号</strong></div></td><td><div align="center"><strong>方法说明 </strong></div></td></tr><tr><td> s.issubset(t)</td><td> s &lt;= t </td><td>子集测试（允许不严格意义上的子集）：s 中所有的元素都是 t 的成员</td></tr><tr><td> </td><td> s &lt; t </td><td>子集测试（严格意义上）：s != t 而且 s 中所有的元素都是 t 的成员</td></tr><tr><td> s.issuperset(t)</td><td> s &gt;= t </td><td>超集测试（允许不严格意义上的超集）：t 中所有的元素都是 s 的成员 </td></tr><tr><td> </td><td> s &gt; t </td><td>超集测试（严格意义上）：s != t 而且 t 中所有的元素都是 s 的成员 </td></tr><tr><td> s.union(t)</td><td> s | t </td><td>合并操作：s &quot;或&quot; t 中的元素 </td></tr><tr><td> s.intersection(t)</td><td> s &amp; t </td><td>交集操作：s &quot;与&quot; t 中的元素 </td></tr><tr><td> s.difference</td><td> s - t </td><td>差分操作：在 s 中存在，在 t 中不存在的元素 </td></tr><tr><td> s.symmetric_difference(t)</td><td> s ^ t </td><td>对称差分操作：s &quot;或&quot; t 中的元素，但不是 s 和 t 共有的元素</td></tr><tr><td> s.copy()</td><td> </td><td>返回 s 的拷贝（浅复制） </td></tr><tr><td><div align="center"><strong> 以下方法仅适用于可变集合</strong></div></td><td> </td><td> </td></tr><tr><td> s.update</td><td> s |= t </td><td>将 t 中的元素添加到 s 中 </td></tr><tr><td> s.intersection_update(t)</td><td> s &amp;= t </td><td>交集修改操作：s 中仅包括 s 和 t 中共有的成员 </td></tr><tr><td> s.difference_update(t)</td><td> s -= t </td><td>差修改操作：s 中包括仅属于 s 但不属于 t 的成员 </td></tr><tr><td> s.symmetric_difference_update(t)</td><td> s ^= t </td><td>对称差分修改操作：s 中包括仅属于 s 或仅属于 t 的成员 </td></tr><tr><td> s.add(obj)</td><td> </td><td>加操作：将 obj 添加到 s </td></tr><tr><td> s.remove(obj)</td><td> </td><td>删除操作：将 obj 从 s 中删除，如果 s 中不存在 obj，将引发异常</td></tr><tr><td> s.discard(obj)</td><td> </td><td>丢弃操作：将 obj 从 s 中删除，如果 s 中不存在 obj，也没事儿^_^</td></tr><tr><td> s.pop()</td><td> </td><td>弹出操作：移除并返回 s 中的任意一个元素 </td></tr><tr><td> s.clear()</td><td> </td><td>清除操作：清除 s 中的所有元素</td></tr></table><br />
<br />
