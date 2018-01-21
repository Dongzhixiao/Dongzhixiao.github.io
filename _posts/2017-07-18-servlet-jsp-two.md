---
layout:     post
title:      "servlet和jsp学习（二）"
subtitle:   "2017/07/18 Servlet Jsp"
date:       2017-07-18
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170717.jpg?raw=true"
tags:
    - Servle
    - Jsp
---

### 时间:2017年7月18日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

##1,http是什么(超文本传输协议)? 
hypertext transfer protocol
    由w3c制订的一种网络应用层协议,它规定了浏览器与web服务器之间如何通信以及通信
所使用的数据格式。

###(1)如何通信
- step1,建立连接
- step2,浏览器将请求数据打包，发送
- step3,web服务器将响应数据打包，发送
- step4,web服务器关闭连接

-------------------------------

- 特点：一次请求，一次连接。
- 优点：web服务器可以利用有限的连接资源为尽可能多的客户端服务。
- 缺点：无状态。

###(2)数据格式
- 1)请求数据包		
	   - a,请求行:请求方式 请求资源路径 协议类型和版本
       - b,若干消息头:请求数据的描述----
        一般是由w3c定义的一些健值对，浏览器与web服务器之间可以通过发送这些消息头来传递一些特定的信息。
        比如，浏览器可以发送"user-agent"消息头，告诉web服务器浏览器的类型和版本。
       - c,实体内容：具体的业务数据----
        只有当发送post请求时，才会有数据(请求参数)。
- 2)响应数据包
       - a,状态行: 协议类型和版本 状态码 状态描述
        注： 状态码是一个三位数字，由w3c定义，表示web服务器处理请求的一种状态。
         - 200: 正确
         - 500: 系统错误
         - 404: 依据请求地址找不到对应的资源
       - b,若干消息头：响应数据的描述----
        服务器也可以发送一些消息头给浏览器，
        比如，"content-type"消息头，告诉浏览器服务器返回的数据类型。
       - c,实体内容：具体的返还数据	
        程序处理的结果

###(3)对开发者的要求
- 1)不用开发者处理的地方
       - 浏览器自动打包请求数据
       - 浏览器自动发送请求数据
       - 服务器自动打包响应数据
       - 服务器自动发送响应数据
- 2)需要开发者处理的地方
       - 提供具体的请求中的业务数据
       - 提供具体的响应中的返还数据
       - 通过ruquest处理请求数据，通过response处理响应数据
        > 开发者会使用request和response就行了

##2.两种请求方式

###(1)get方式
- 1)哪一些情况下，会发送get请求
    - a,直接输入某个地址
    - b,点击链接
    - c,表单默认提交方式 
- 2)get请求的特点
    - a,会将请求参数添加到请求资源路径的后面，只能提交少量的数据(因为请求行最多只能存放大约2k左右的数据)
    - b,会将请求参数显示在浏览器地址栏，不安全，比如，路由器会记录请求地址。
- 3)所有请求默认都是get请求

###(2)post方式	
- 1)采用实体内容传参
- 2)操作方式：在form上面加上属性method="post"。
- 2)post请求的特点
    - a,会将请求参数添加到实体内容里面，可以提交大量的数据。
    - b,不会将请求参数显示在浏览器地址栏，相对安全(要注意，不管是什么请求，都不会对请求数据加密,一般使用https协议)。

##3.servlet中文乱码问题

###(1)传入服务器的内容由乱码
>为什么会有乱码？----当表单提交时，浏览器会检查请求参数值，如果是中文，会按照打开该表单所在的页面时的字符集来编码(比如，按照"utf-8"来编码)。
服务器默认情况下，会使用"iso-8859-1"来解码。

####解决方案：
- setp1: 保证浏览器使用指定的字符集来打开页面。<meta http-equiv="content-type" content="text/html;charset=utf-8">
(注意setp1和下面的step2没有内在联系，setp1是默认都这样做)
- setp2: 服务器端使用对应的字符集去解码。
    - 1)在接收到内容后先反解码，再解码，比如浏览器是utf-8编码形式，我们按照ISO8859-1解码了，就需要以下代码
    ```
        byte[] bs = userName.getBytes("ISO8859-1");//按照ISO8859-1反解码
        userName = new String(bs,"utf-8");//按照utf-8编码
    ```
    - 2)在Tomcat的配置文件server.xml里面的<Connector>标签内加入属性URIEncoding="utf-8"
        - 优点：简单
        - 缺点，只对get访问有效(因为get是路径编码形式，post就不是啦)
    - 3)在接收参数前加`req.setCharacterEncoding("utf-8");//声明实体内容的编码为utf-8`
        - 优点：简单
        - 缺点：只对post访问有效
    ** 总结：setp2里面的2)和3)结合使用！ **
    
###(2)客户端接收的网页有乱码
>为什么会有乱码？----因为out.println输出中文时，默认会使用"ISO8859-1"去编码。		
	
####解决方案：
- response.setCharacterEncoding("utf-8");//让服务器使用utf-8编码
- response.setContentType("text/html;charset=utf-8");//浏览器使用utf-8解码
    注：上面两句话写一句即可，另一个会默认相同！一般选择写第二个！！    
    
##4.常见的错误及处理方式

###(1)404
- 1)错误原因:
    - a,应用没有部署。
    - b,请求地址写错。
        按照http://ip:port/appname/url-pattern
    - c,<servlet-name>不一致。
###(2)500
- 1)错误原因
       - a,程序运行时出错。
       - b,<servlet-class>写错。
###(3)405
- 1)错误原因
        服务器找不到处理方法。
			
##5.如何获得请求参数值?

###(1)String request.getParameter(String paramName);
注意：

- a,paramName必须与实际传递的参数名一致，否则会获得null。
- b,有可能获得空字符串。
    
###(2)String[] request.getParameterValues(String paramName);
对于多选框和单选框，如果不选择任何选项，会获得null值。
	
##6.servlet如何使用jdbc来访问数据库
- step1,将jdbc驱动拷贝到WEB-INF\lib下。
	注：
		服务器一般都提供了自己的类加载器(比如tomcat
	就提供了自己的类加载器),这些类加载器会从
	WEB-INF\lib下查找字节码文件。
- step2,在servlet类里面，使用jdbc 提供的
	方法来访问数据库,要注意异常的处理。
	
##7.mysql数据库的简单使用
- (1)登录mysql
    mysql -uroot;  root用户登录mysql数据库管理系统
- (2)查看当前有哪些数据库
    show databases;
- (3)创建一个新的数据库(同时设置字符集为utf-8)
    create database jsd1407
    default character set utf8;
- (4)使用指定的数据库
		use jsd1407;
- (5)查看当前数据库有哪些表
		show tables;
- (6)建表
  ```
    create table emp(
        id int primary key auto_increment,
        name varchar(50),
        salary double,
        age int
    );	
    insert into emp(name,salary,age) 
    values('Sally',20000,32);
    ```
    auto_increment: 自增长列，当插入记录时，
    数据库自动为该列赋一个自动增长的值。
	
		

