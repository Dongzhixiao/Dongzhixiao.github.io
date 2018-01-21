---
layout:     post
title:      "servlet和jsp学习（一）"
subtitle:   "2017/07/17 Servlet Jsp"
date:       2017-07-17
author:     "WangXiaoDong"
header-img: "https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/20170717.jpg?raw=true"
tags:
    - Servle
    - Jsp
---

### 时间:2017年7月17日 天气:晴
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
----------

##1,网络应用程序的架构(了解)

###(1)主机/终端
- 特点：
	主机负责所有的计算(处理业务),终端只负责输入输出(不做任何计算)。
- 优点：
	可靠,安全,i/o能力强。
- 缺点:
	昂贵,扩展困难。

###(2)client/server
![client/server](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/cs.jpg?raw=true "client/server图像")

###1)两层的client/server
- 特点：
	使用数据库来充当服务器(大量的业务处理逻辑是使用数据库特定的编程语言来写的)。客户端提供界面和少量的业务逻辑处理。
- 缺点：
	可移植性差(特定的编程语言)。
	不适合大型应用(要求客户端与
	数据库服务器建立一个持续连接)。

###2)三层的client/server
- 特点：
	所有的业务处理都由应用服务器来做。
- 优点：
	可移值性好(一般应用服务器都是由
	java语言来写的)。
	适合开发大型的应用。
- 缺点:
	客户端需要单独安装和维护。
	开发复杂。

###(3)browser/web server
![browser/web server](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/bs.jpg?raw=true "browser/web server图像")

- 特点：
	使用浏览器来充当客户端，使用web服务器来充当应用服务器，使用标准化的http协议来通信。
- 优点：
	开发相对简对(不需要开发通信模块，
	不需要自定义协议)。
	不需要单独安装客户端了。

##2,什么是servlet?
sun公司制订的一种用来扩展web服务器功能的组件规范。
servlet = server(服务器) + let(小程序)

###(1)扩展web服务器功能		
web服务器收到请求之后，可以调用servlet来处理。

###(2)组件规范
- a,什么是组件?
	符合规范，并且实现了部分功能的软件模块。
	组件一般需要部署到相应的容器里面才能运行。
- b,什么是容器?
	符合规范，提供组件的运行环境的程序。
	tomcat提供了一个servlet容器。
>注：
	有些web服务器，比如iis,apache httpserver等没有提供servlet容器，需要另外再嵌入或者调用单独的servlet容器才能运行servlet。
	组件不依赖特定的容器的，比如，
	我们开发了一个servlet,可以部署到weblogic, jetty,was，tomcat等服务器上运行。

##3,如何写一个servlet?
- step1,写一个java类，实现Servlet
	接口或者继承HttpServlet抽象类。
- step2,编译。
- step3,打包，即创建一个具有如下结构的文件夹
>appname
>>WEB-INF
>>>classes(放.class文件)
lib(可选,放.jar文件)
web.xml(部署描述文件)

- step4,部署
		将step3创建好的整个文件夹拷贝到服务器特定的文件夹下面，比如,tomcat
		一般可以拷贝到webapps下面。
- step5,启动服务器，访问
	`	http://ip:port/appname/url-pattern`
	`	http://localhost:8080/web01/hello`
    > 如果出错，可能是端口被占用，可以修改端口：8088 8089	
    
##4,安装tomcat
- step1,将tomcat的安装压缩文件解压到/home/soft01下。
	(www.apache.org下载tomcat)
- step2,配置环境变量。
		JAVA_HOME: jdk安装路径
		CATALINA_HOME:tomcat的安装路径,
		比如 /home/soft01/apache-tomcat7
		PATH: tomcat的安装路径/bin
- step3,启动tomcat
```
cd /home/soft01/apache-tomcat7/bin
sh startup.sh(或者sh catalina.sh run)
 	  	     (windows环境下用startup.bat)
```
- step4,访问tomcat
	打开浏览器，输入
	`http://localhost:8080`
    >关闭tomcat：打开/tomcat/bin,使用里面的shutdown命令
##5,使用myeclipse来开发servlet
- step1,启动myeclipse,配置tomcat。
- step2,创建一个web工程。
    >src/main/webapp/WEB-INF/web.xml(必须结构)，否则错误，因此需要使用右键项目名，

- step3,导入jar包
	- 1)使用marven搜索javaee,在结果中选择javaee-api
	- 2)使用tomcat自带包
	    - 选择项目，右键点击properties		
	    - 弹出框里在左侧选择Targeted Runtimes
	    - 右侧勾选Apache Tomcat			
- step4,开发Servlet
	- 1)编写Servlet
	    - 创建package
	    - 创建一个类名为XxxServlet
	    - 继承HttpServlet，从而间接的实现了Servlet的接口
	    - 重写父类的service()方法
    - 2)配置Servlet
	    - 声明类，给它取别名
	    - 通过其别名引用此类，给它取一个访问路径
- step5,部署
    - 在Servers视图下，选择tomcat7
    - 右键点击Add and Remove
    - 在弹出框内将左边的待部署项目移动到右侧
    - 启动tomcat即可
- step6,访问tomcat
    - http://ip:port/项目名/网名
    - eg: http://192.168.1.102:8080/servlet1/ts
   
##6,servlet是如何运行的?
比如，在浏览器地址栏输入
`http://ip:port/firstweb/hello?uname=tom`

- step1,浏览器依据ip,port建立连接。
- step2,浏览器将相关数据(比如，请求资源路径,请求参数)打包,然后发送请求。
- step3,服务器解析请求数据包,然后将解析之后的数据保存到request对象上，同时，服务器还会创建一个response对象。
- step4,服务器创建servlet对象，然后调用该对象的service方法(服务器会将之前创建好的request,response作为参数传递进来)。
- step5,在service方法里面，通过request对象来获得请求数据(比如，请求参数值),然后进行相应的处理，最后将处理结果写到response对象上。
- step6,服务器从response对象上取之前保存的处理结果，然后打包，发送给浏览器。
- step7,浏览器解析响应数据包，然后依据解析的结果生成新的页面。

 整体访问过程如下所示：
![browser/web server](https://github.com/Dongzhixiao/PictureCache/blob/master/diaryPic/servlet.jpg?raw=true "browser/web server图像")
