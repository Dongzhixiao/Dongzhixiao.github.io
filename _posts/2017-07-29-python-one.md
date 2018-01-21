---
layout:     post
title:      "python学习（一）"
subtitle:   "2017/07/29 os模块中常用方法总结"
date:       2017-07-29
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年7月29日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

## os模块中关于文件/目录常用的函数使用方法
<table cellspacing="0" class="t_table" style="width:98%"><tr><td><div align="center"><strong><font style="font-size:12.0pt">函数名</font></strong></div></td><td><div align="center"><strong><font style="font-size:12.0pt">使用方法</font></strong></div></td></tr><tr><td><font style="font-size:12.0pt">getcwd()</font><br />
</td><td><font style="font-size:12.0pt">返回当前工作目录</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">chdir(path)</font><br />
</td><td><font style="font-size:12.0pt">改变工作目录</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">listdir(path='.')</font><br />
</td><td><font style="font-size:12.0pt">列举指定目录中的文件名（'.'表示当前目录，'..'表示上一级目录）</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">mkdir(path)</font><br />
</td><td><font style="font-size:12.0pt">创建单层目录，如该目录已存在抛出异常</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">makedirs(path)</font><br />
</td><td><font style="font-size:12.0pt">递归创建多层目录，如该目录已存在抛出异常，注意：'E:\\a\\b'和'E:\\a\\c'并不会冲突</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">remove(path)</font><br />
</td><td><font style="font-size:12.0pt">删除文件</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">rmdir(path)</font><br />
</td><td><font style="font-size:12.0pt">删除单层目录，如该目录非空则抛出异常</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">removedirs(path)</font><br />
</td><td><font style="font-size:12.0pt">递归删除目录，从子目录到父目录逐层尝试删除，遇到目录非空则抛出异常</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">rename(old, new)</font><br />
</td><td><font style="font-size:12.0pt">将文件old重命名为new</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">system(command)</font><br />
</td><td><font style="font-size:12.0pt">运行系统的shell命令</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">walk(top)</font><br />
</td><td><font style="font-size:12.0pt">遍历top路径以下所有的子目录，返回一个三元组：(路径, [包含目录], [包含文件])</font><br />
</td></tr><tr><td colspan="2" width="553"><div align="center"><i><font style="font-size:12.0pt">以下是支持路径操作中常用到的一些定义，支持所有平台</font></i></div></td></tr><tr><td><font style="font-size:12.0pt">os.curdir</font><br />
</td><td><font style="font-size:12.0pt">指代当前目录（'.'）</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">os.pardir</font><br />
</td><td><font style="font-size:12.0pt">指代上一级目录（'..'）</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">os.sep</font><br />
</td><td><font style="font-size:12.0pt">输出操作系统特定的路径分隔符（Win下为'\\'，Linux下为'/'）</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">os.linesep</font><br />
</td><td><font style="font-size:12.0pt">当前平台使用的行终止符（Win下为'\r\n'，Linux下为'\n'）</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">os.name</font><br />
</td><td><font style="font-size:12.0pt">指代当前使用的操作系统（包括：'posix',&nbsp;&nbsp;'nt', 'mac', 'os2', 'ce', 'java'）</font><br />
</td></tr></table>
##os.path模块中关于路径常用的函数使用方法
<table cellspacing="0" class="t_table" style="width:98%"><tr><td><div align="center"><strong><font style="font-size:12.0pt">函数名</font></strong></div></td><td><div align="center"><strong><font style="font-size:12.0pt">使用方法</font></strong></div></td></tr><tr><td><font style="font-size:12.0pt">basename(path)</font><br />
</td><td><font style="font-size:12.0pt">去掉目录路径，单独返回文件名</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">dirname(path)</font><br />
</td><td><font style="font-size:12.0pt">去掉文件名，单独返回目录路径</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">join(path1[, path2[, ...]])</font><br />
</td><td><font style="font-size:12.0pt">将path1, path2各部分组合成一个路径名</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">split(path)</font><br />
</td><td><font style="font-size:12.0pt">分割文件名与路径，返回(f_path, f_name)元组。如果完全使用目录，它也会将最后一个目录作为文件名分离，且不会判断文件或者目录是否存在</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">splitext(path)</font><br />
</td><td><font style="font-size:12.0pt">分离文件名与扩展名，返回(f_name, f_extension)元组</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">getsize(file)</font><br />
</td><td><font style="font-size:12.0pt">返回指定文件的尺寸，单位是字节</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">getatime(file)</font><br />
</td><td><font style="font-size:12.0pt">返回指定文件最近的访问时间（浮点型秒数，可用time模块的gmtime()或localtime()函数换算）</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">getctime(file)</font><br />
</td><td><font style="font-size:12.0pt">返回指定文件的创建时间（浮点型秒数，可用time模块的gmtime()或localtime()函数换算）</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">getmtime(file)</font><br />
</td><td><font style="font-size:12.0pt">返回指定文件最新的修改时间（浮点型秒数，可用time模块的gmtime()或localtime()函数换算）</font><br />
</td></tr><tr><td colspan="2" width="553"><div align="center"><i><font style="font-size:12.0pt">以下为函数返回 True 或 False</font></i></div></td></tr><tr><td><font style="font-size:12.0pt">exists(path)</font><br />
</td><td><font style="font-size:12.0pt">判断指定路径（目录或文件）是否存在</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">isabs(path)</font><br />
</td><td><font style="font-size:12.0pt">判断指定路径是否为绝对路径</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">isdir(path)</font><br />
</td><td><font style="font-size:12.0pt">判断指定路径是否存在且是一个目录</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">isfile(path)</font><br />
</td><td><font style="font-size:12.0pt">判断指定路径是否存在且是一个文件</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">islink(path)</font><br />
</td><td><font style="font-size:12.0pt">判断指定路径是否存在且是一个符号链接</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">ismount(path)</font><br />
</td><td><font style="font-size:12.0pt">判断指定路径是否存在且是一个挂载点</font><br />
</td></tr><tr><td><font style="font-size:12.0pt">samefile(path1, paht2)</font><br />
</td><td><font style="font-size:12.0pt">判断path1和path2两个路径是否指向同一个文件</font><br />
</td></tr></table>

