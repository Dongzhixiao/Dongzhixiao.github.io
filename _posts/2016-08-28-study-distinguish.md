---
layout:     post
title:      "和姐姐通话"
subtitle:   "2016/08/28 感受工作和上学的区别"
date:       2016-08-28
author:     "WangXiaoDong"
header-img: "img/20160828.jpg"
tags:
    - 日记
    - Effect C++
---

### 时间:2016年8月28日 天气:晴:sunny:
-----
#####   Author:冬之晓:angry:
#####   Email: 347916416@qq.com
#####   MyAppearance: ![MyAppearance](https://github.com/Dongzhixiao/PictureCache/raw/master/MyPicture.JPG "我的头像")
----------

<pre>
    今天晚上，田田姐姐给我打电话啦，感觉很高兴，我说了我在这边最大的缺点就是回家不方便，并且感到孤独。
	确实，在学校的时候没有这种感觉，因为周围的同学有很多，大家的年龄都差不多，状态都是一样的，
	可是出来工作周围的同事，各个年龄段的人都有，工作时间的长短也都不一样。感到非常的不适应，
	非常怀念在学校读书的时候。不过在这里工作同事们也都很好，相互关心，共同进步……
</pre>

#### 条款14：在资源管理类中小心coping行为

在前一个条款，我们提出了RAII（资源获得即是初始化）技术，通过“对象管理资源”达到防止资源泄露，对于通过堆分配的内存，可以借助指针指针实现，但系统中有很多资源不是堆分配：文件句柄，锁，网络套接字。这些资源就需要自己实现对象来管理。
看一个简单的实现互斥锁资源对象管理：

```C++
class Mutex{};
void lock(Mutex* mutex) {}  //锁住资源
void unlock(Mutex* mutex) {} //释放资源

class MyLock
{
public:
	explicit MyLock(Mutex *mutex) :m_mutex(mutex)
	{
		lock(m_mutex);
	}

	~MyLock() {unlock(m_mutex);}
private:
	Mutex *m_mutex;
};

void process()
{
	Mutex mutex;
	MyLock mylock(&mutex);
	//process
} //离开作用域，mylock执行析构函数，释放mutex
```

管理对象被复制了，会出现什么情况？

```C++
void process()
{
	Mutex mutex;
	MyLock mylock(&mutex);
	MyLock mylock2(mylock);  //调用默认拷贝构造函数，指向同一个资源
	//process
} //离开作用域，mylock和mylock2执行析构函数，释放mutex两次
```

如何解决RAII对象被复制的情况呢？
①禁止复制；可以通过条款6的方法禁止类对象复制

```C++
//可以私有化复制构造函数
class MyLock : private Uncopyable {};

//也可以使用C++11的语法
class MyLock : Uncopyable() = delete;
```

②对底层使用“引用计数法”
使用shared\_ptr成员变量，指向需要管理的资源，当引用计数为0时则delete掉资源，但非堆分配内存是不能delete操作，只需要释放，为此，shared\_ptr提供一种方法，即删除器，在引用计数为0时执行

```C++
class Mutex{};
void lock(Mutex* mutex) {}  //锁住资源
void unlock(Mutex* mutex) {} //释放资源

class MyLock
{
public:
	explicit MyLock(Mutex *mutex) :m_mutexPtr(mutex, unlock)
	{
		lock(m_mutexPtr.get());
	}

	//~MyLock() {unlock(m_mutex);} //不用再定义析构函数，直接通过m_mutexPtr删除器释放资源
private:
	std::tr1::shared_ptr<Mutex> m_mutexPtr;

};

void process()
{
	Mutex mutex;
	MyLock mylock(&mutex);   //m_mutexPtr引用计数为2
	MyLock mylock2(mylock);  //调用默认拷贝构造函数，指向同一个资源,m_mutexPtr引用计数为2
	//process

} //离开作用域，成员对象mylock和mylock2的智能指针对象都执行析构函数且引用计数减为0，则调用删除器unlock()函数解除锁
```


③复制底部资源
有些时候可以对某些资源进行多份拷贝，如常用的string类，实现方法是前面说过的深拷贝
④转移底部资源的拥有权。
前面说过的auto_ptr就是采用这种技术.

请记住：

- [x] ***复制RAII对象必须一并复制它所管理的资源，所以资源copying行为决定RAII对象的copying行为***
- [x] ***普遍而常见的RAII class copying行为是：抑制copying，施行引用计数法（shared_ptr思想），或者是转移底部资源（auto_ptr思想）***
