---
layout:     post
title:      "C语言可变参数"
subtitle:   "2017/07/03 可变参数"
date:       2017-07-03
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170703.jpg?raw=true"
tags:
    - C
---

### 时间:2017年7月3日 天气:阴:cloud:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

最近发现C语言可以使用可变参数，挺有意思的，下面就举一个例子：

```C
    #include <stdio.h>
#include <stdarg.h>
int max(int cnt, ...)
{
    va_list v;//v保存可变长参数表,va_list是个类型
    va_start(v,cnt);//用v保存参数cnt之后的那些参数
    int i;
    int maxvalue = va_arg(v, int);//从参数表中取出一个int类型的参数
    for(i=1; i<cnt; i++){
        int data = va_arg(v, int);//从参数表中取出一个int类型的参数
        if(data>maxvalue)
            maxvalue = data;
    }
    va_end(v);//释放可变长参数表v
    return maxvalue;
}
void printchar(int cnt,...)
{
    va_list v;
    va_start(v, cnt);
    int i;
    for(i=0; i<cnt; i++)
        printf("%c", va_arg(v, int));//char,short=>int
    printf("\n");                    //float=>double
    va_end(v);
}
void printstring(int cnt,...)
{
    va_list v;
    va_start(v, cnt);
    int i;
    for(i=0; i<cnt; i++)
        puts(va_arg(v, char*));
    va_end(v);
}
int main()
{
    printf("%d\n", max(2,88,69));
    printf("%d\n", max(5,91,25,86,97,89));
    printf("%d\n", max(4,93,65,76,87));
    printchar(3, 'a','b','c');
    printchar(5, 'd','e','f','g','h');
    printstring(3,"hello","my","dear");
    printstring(2,"good","afternoon");
    return 0;
}

```