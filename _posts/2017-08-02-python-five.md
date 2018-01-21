---
layout:     post
title:      "python学习（五）"
subtitle:   "2017/08/02 Python标准异常总结"
date:       2017-08-02
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170729.jpg?raw=true"
tags:
    - Python
---

### 时间:2017年8月2日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

## Python标准异常总结
<table cellspacing="0" class="t_table" style="width:500px"><tr><td> AssertionError</td><td> 断言语句（assert）失败</td></tr><tr><td> AttributeError</td><td> 尝试访问未知的对象属性</td></tr><tr><td> EOFError</td><td>用户输入文件末尾标志EOF（Ctrl+d）</td></tr><tr><td> FloatingPointError</td><td> 浮点计算错误</td></tr><tr><td> GeneratorExit</td><td> generator.close()方法被调用的时候</td></tr><tr><td> ImportError</td><td> 导入模块失败的时候</td></tr><tr><td> IndexError</td><td> 索引超出序列的范围</td></tr><tr><td> KeyError</td><td> 字典中查找一个不存在的关键字</td></tr><tr><td> KeyboardInterrupt</td><td> 用户输入中断键（Ctrl+c）</td></tr><tr><td> MemoryError</td><td> 内存溢出（可通过删除对象释放内存）</td></tr><tr><td> NameError</td><td> 尝试访问一个不存在的变量</td></tr><tr><td> NotImplementedError</td><td> 尚未实现的方法</td></tr><tr><td> OSError</td><td> 操作系统产生的异常（例如打开一个不存在的文件）</td></tr><tr><td> OverflowError</td><td> 数值运算超出最大限制</td></tr><tr><td> ReferenceError</td><td> 弱引用（weak reference）试图访问一个已经被垃圾回收机制回收了的对象</td></tr><tr><td> RuntimeError</td><td> 一般的运行时错误</td></tr><tr><td> StopIteration</td><td> 迭代器没有更多的值</td></tr><tr><td> SyntaxError</td><td> Python的语法错误</td></tr><tr><td> IndentationError </td><td> 缩进错误</td></tr><tr><td> TabError</td><td> Tab和空格混合使用</td></tr><tr><td> SystemError</td><td> Python编译器系统错误</td></tr><tr><td> SystemExit</td><td> Python编译器进程被关闭</td></tr><tr><td> TypeError</td><td> 不同类型间的无效操作</td></tr><tr><td> UnboundLocalError</td><td>访问一个未初始化的本地变量（NameError的子类）</td></tr><tr><td> UnicodeError</td><td>Unicode相关的错误（ValueError的子类）</td></tr><tr><td> UnicodeEncodeError</td><td>Unicode编码时的错误（UnicodeError的子类）</td></tr><tr><td>UnicodeDecodeError</td><td>Unicode解码时的错误（UnicodeError的子类）</td></tr><tr><td>UnicodeTranslateError</td><td>Unicode转换时的错误（UnicodeError的子类）</td></tr><tr><td>ValueError</td><td>传入无效的参数</td></tr><tr><td> ZeroDivisionError</td><td> 除数为零</td></tr></table><br />
<br />
<strong>以下是 Python 内置异常类的层次结构：</strong><br />
<br />
BaseException<br />
 +-- SystemExit<br />
 +-- KeyboardInterrupt<br />
 +-- GeneratorExit<br />
 +-- Exception<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- StopIteration<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- ArithmeticError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- FloatingPointError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- OverflowError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- ZeroDivisionError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- AssertionError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- AttributeError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- BufferError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- EOFError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- ImportError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- LookupError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- IndexError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- KeyError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- MemoryError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- NameError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- UnboundLocalError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- OSError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- BlockingIOError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- ChildProcessError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- ConnectionError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; |&nbsp; &nbsp; +-- BrokenPipeError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; |&nbsp; &nbsp; +-- ConnectionAbortedError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; |&nbsp; &nbsp; +-- ConnectionRefusedError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; |&nbsp; &nbsp; +-- ConnectionResetError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- FileExistsError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- FileNotFoundError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- InterruptedError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- IsADirectoryError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- NotADirectoryError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- PermissionError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- ProcessLookupError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- TimeoutError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- ReferenceError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- RuntimeError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- NotImplementedError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- SyntaxError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- IndentationError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;+-- TabError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- SystemError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- TypeError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- ValueError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp; +-- UnicodeError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;+-- UnicodeDecodeError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;+-- UnicodeEncodeError<br />
&nbsp; &nbsp;&nbsp; &nbsp;|&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;+-- UnicodeTranslateError<br />
&nbsp; &nbsp;&nbsp; &nbsp;+-- Warning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- DeprecationWarning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- PendingDeprecationWarning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- RuntimeWarning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- SyntaxWarning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- UserWarning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- FutureWarning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- ImportWarning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- UnicodeWarning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- BytesWarning<br />
&nbsp; &nbsp;&nbsp; &nbsp;&nbsp; &nbsp;&nbsp;&nbsp;+-- ResourceWarning<br />
<br />
</td></tr></table>