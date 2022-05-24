---
title: C++析构函数为什么要为虚函数

date: 2019-10-09 08:57:08

categories: 笔记

tags: [C++,析构函数,虚函数]
---


# [C++析构函数为什么要为虚函数](https://hokori.cn/2019/10/09/C++%E6%9E%90%E6%9E%84%E5%87%BD%E6%95%B0%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E4%B8%BA%E8%99%9A%E5%87%BD%E6%95%B0/)

**注：本文内容来源于zhice163博文，感谢作者的整理。**

1.为什么基类的析构函数是虚函数？

　　在实现多态时，当用基类操作派生类，在析构时防止只析构基类而不析构派生类的状况发生。

　　下面转自网络：源地址 [http://blog.sina.com.cn/s/blog_7c773cc50100y9hz.html](http://blog.sina.com.cn/s/blog_7c773cc50100y9hz.html)

a.第一段代码

```cpp
#include <iostream>
using namespace std;
class ClxBase{
public:
    ClxBase() {};
    ~ClxBase() {cout << "Output from the destructor of class ClxBase!" << endl;};
 
    void DoSomething() { cout << "Do something in class ClxBase!" << endl; };
};
 
class ClxDerived : public ClxBase{
public:
    ClxDerived() {};
    ~ClxDerived() { cout << "Output from the destructor of class ClxDerived!" << endl; };
 
    void DoSomething() { cout << "Do something in class ClxDerived!" << endl; };
};
  int   main(){  
  ClxDerived *p =  new ClxDerived;
  p->DoSomething();
  delete p;
  return 0;
  }
```

　　运行结果：

　　Do something in class ClxDerived!            

　　Output from the destructor of class ClxDerived!

　　Output from the destructor of class ClxBase!  

　　这段代码中基类的析构函数不是虚函数,在main函数中用继承类的指针去操作继承类的成员,释放指针P的过程是:先释放继承类的资源,再释放基类资源. 

　　b.第二段代码

```cpp
#include<iostream>

using namespace std;
class ClxBase{
public:
    ClxBase() {};
    ~ClxBase() {cout << "Output from the destructor of class ClxBase!" << endl;};
 
    void DoSomething() { cout << "Do something in class ClxBase!" << endl; };
};
 
class ClxDerived : public ClxBase{
public:
    ClxDerived() {};
    ~ClxDerived() { cout << "Output from the destructor of class ClxDerived!" << endl; };
 
    void DoSomething() { cout << "Do something in class ClxDerived!" << endl; }
};
  int   main(){  
  ClxBase *p =  new ClxDerived;
  p->DoSomething();
  delete p;
  return 0;
  } 
```

　　输出结果：

　　Do something in class ClxBase!
　　Output from the destructor of class ClxBase!

_这段代码中基类的析构函数同样不是虚函数,不同的是在main函数中用基类的指针去操作继承类的成员,释放指针P的过程是:只是释放了基类的资源,而没有调用继承类的析构函数.调用dosomething()函数执行的也是基类定义的函数._

_一般情况下,这样的删除只能够删除基类对象,而不能删除子类对象,形成了删除一半形象,造成内存泄漏._

_在公有继承中,基类对派生类及其对象的操作,只能影响到那些从基类继承下来的成员.如果想要用基类对非继承成员进行操作,则要把基类的这个函数定义为虚函数._

_析构函数自然也应该如此:如果它想析构子类中的重新定义或新的成员及对象,当然也应该声明为虚的._

　　c.第三段代码：
　　

```cpp
#include<iostream>

using namespace std;
class ClxBase{
public:
    ClxBase() {};
    virtual ~ClxBase() {cout << "Output from the destructor of class ClxBase!" << endl;};
    virtual void DoSomething() { cout << "Do something in class ClxBase!" << endl; };
};
 
class ClxDerived : public ClxBase{
public:
    ClxDerived() {};
    ~ClxDerived() { cout << "Output from the destructor of class ClxDerived!" << endl; };
    void DoSomething() { cout << "Do something in class ClxDerived!" << endl; };
};
 
  int   main(){  
  ClxBase *p =  new ClxDerived;
  p->DoSomething();
  delete p;
  return 0;
  }  
```

　　运行结果：

　　Do something in class ClxDerived!
　　Output from the destructor of class ClxDerived!
　　Output from the destructor of class ClxBase!

_这段代码中基类的析构函数被定义为虚函数,在main函数中用基类的指针去操作继承类的成员,释放指针P的过程是:只是释放了继承类的资源,再调用基类的析构函数.调用dosomething()函数执行的也是继承类定义的函数。_

 _如果不需要基类对派生类及对象进行操作,则不能定义虚函数,因为这样会增加内存开销.当类里面有定义虚函数的时候,编译器会给类添加一个虚函数表,里面来存放虚函数指针,这样就会增加类的存储空间.所以,只有当一个类被用来作为基类的时候,才把析构函数写成虚函数。_
