---
layout:     post
title:      "Vector::erase() 解析"
subtitle:   "erase()删除一个元素时，到底发生了什么"
date:       2019-08-07 15:00:01
author:     "Chen"
header-img: "img/post-default-header.jpg"
catalog: true
tags:
    - 编程
---

> “一个人行走的范围，就是他的世界 ”



vector 是c++程序员最常用的容器，但是最近在用vector的erase()函数时，产生了一个疑问：

1.  erase()函数调用之后，整个容器的容量大小会不会发生改变？
2.  erase()里面到底发生了什么？

所以找到了SGI STL的实现源码，内容很简单：

// 清除某个位置上的元素
iterator erase(iterator __position) {
	if (__position + 1 != end())
		copy(__position + 1, _M_finish, __position); // 全局函数
	--_M_finish;
	destroy(_M_finish);
	return __position;
}`

但是真正的问题来了：erase()里面为什么调用destroy()去析构最后一个元素(M_finish)，而不是析构__position这个位置的元素？

上网搜索到了一篇博文（https://blog.csdn.net/evilswords/article/details/8930918）也验证了我的猜想。简单来说就是假如vector存储了如下序列：

![](/img/in-post/open-source-license.png)

A0~A3分别代表一个类的四个对象，那么当 erase(pos)时，会调用A3的析构函数。

看起来是个bug，但后来和同事讨论，发现这不能算是一个bug。因为在上述erase()源码中，copy()函数会逐个调用**赋值函数**（operator =），所以当删除A1时，A1还在，不过里面的值变成A2的值了。同理A2中的值变成了A3，所以最后空出一个A3来，必须析构掉。

简单写了一个测试程序，如下：

`#include <iostream>`
`#include <vector>`
`using namespace std;`
`class A {`
`public:`
	`A(int x) : value(x) {}`
	`A(const A &a) {`
		`value = a.value;`
		`cout << "拷贝构造函数" << value << endl;`
	`}`
	`A& operator=(const A& a) {`
		`value = a.value;`
		`cout << "赋值构造函数" << value << endl;`
		`return *this;`
	`}`
	`~A() {`
		`cout << "调用析构函数" << value <<endl;`
	`}`
`private:`
	`int value = 0;`
`};`
`int main()`
`{`
	`vector<A> vec;`
	`//vec.reserve(4);`
	`A a(1), b(2), c(3);`
	`vec.push_back(a);`
	`vec.push_back(b);`
	`vec.push_back(c);`
	`cout << "--------------------" << endl;`
	`vec.erase(vec.begin());`
	`system("pause");`
	`return 0;`
`}`

结果如下：

![](/img/in-post/post-c-u-ali-team.png)

—— Chen


