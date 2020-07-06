# C++ STL

## STL概述

### **六大组件**

- 空间适配器allocator
- 迭代器iterators与traits编程技巧
- 容器container：序列式容器、关联式容器
- 算法algorithms
- 仿函数或函数对象functors, or functions objects
- 配接器adapter

**1.容器container**

各种数据结构，vector、list、deque、set、map用来存放数据。从实现角度来看，STL容器是一种class template 

**2. 算法algorithms**

各种常用算法的实现

**3.迭代器iterators**

  扮演容器与算法之间的胶合剂，是所谓的"泛型指针"。共有五种类型以及它的衍生变化。从实现的角度来看，迭代器是一种将operator*、operator->、operator++、operator--等指针相关操作予以重载的一种class template。所有的容器都附带有自己专属的迭代器，只有容器设计者才能知道怎么遍历自己的容器。原生指针native pointer也是一种迭代器。

**4.仿函数functors**

   类似于行为函数，可作为算法的某种实现策略。从实现角度来看，仿函数重载了一种operator{}的class或class template。一般函数指针可视为狭义的仿函数。

示例： 针对某个class 进行operator()重载，它就成为一个仿函数。

**5.配接器adapters**

  一种用来修饰容器container或仿函数functors或迭代器iterators接口的东西。例如STL提供的queue和stack看似容器，其实只能算一种容器配接器，因为它们的底层完全借助deque、所有的操作均由底层的deque供应。改变functor接口者，称为functor adapters；改变container者，称为container adapters；改变iterators者，称为iterator adapters。

**6.配置器allocators（分配器）**

  负责空间配置与管理。从实现角度来看，配置器是一个实现了动态空间配置、空间管理、空间释放的class template。

### **六大组件的交互关系**

​      container通过allocator取得数据存储空间，algorithm通过iterator获取container内容，functor可以协助algorithm完成不同的策略变化，adapter可以修饰或套接functor。

## **配置器allocator**

  负责空间配置与管理。从实现角度来看，配置器是一个实现了动态空间配置、空间管理、空间释放的class template。

**头文件与关键函数**

| **头文件**        | **作用**                 | **关键函数**                                                 |
| ----------------- | ------------------------ | ------------------------------------------------------------ |
| <memory\>         | allocator定义位置        |                                                              |
| <stl_alloc.h>     | 负责内存空间的配置与释放 | allocate()、deallocate()reallocate()、refill()  malloc()、free() |
| <stl_construct.h> | 负责对象内容的构造与析构 | construct()、destory()                                       |

 <img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200625214917113.png" alt="image-20200625214917113" style="zoom: 67%;" />![image-20200625215030136](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200625215030136.png)

 

### SGI 空间配置器alloc

### 构造和析构基本工具construct()和destroy()



### C++ STL的内存优化与内存池的实现 **——二级配置器结构**

STL内存管理使用二级内存配置器。

#### **1 第一级配置器__malloc_alloc_template**

第一级配置器以malloc()，free()，realloc()等C函数执行实际的内存配置、释放、重新配置等操作，并且能在内存需求不被满足的时候，调用一个指定的函数。

一级空间配置器分配的是大于128字节的空间

如果分配不成功，调用句柄释放一部分内存

如果还不能分配成功，抛出异常

#### **2 第二级配置器__default_alloc_template**

在STL的第二级配置器中多了一些机制，避免太多小区块造成的内存碎片，小额区块带来的不仅是内存碎片，配置时还有额外的负担。区块越小，额外负担所占比例就越大。

#### **3 分配原则**

如果要分配的区块大于128bytes，则移交给第一级配置器处理。

如果要分配的区块小于128bytes，则以内存池管理（memory pool），又称之次层配置（sub-allocation）：每次配置一大块内存，并维护对应的16个空闲链表（free-list）。下次若有相同大小的内存需求，则直接从free-list中取。如果有小额区块被释放，则由配置器回收到free-list中。

当用户申请的空间小于128字节时，将字节数扩展到8的倍数，然后在自由链表中查找对应大小的子链表

如果在自由链表查找不到或者块数不够，则向内存池进行申请，一般一次申请20块

如果内存池空间足够，则取出内存

如果不够分配20块，则分配最多的块数给自由链表，并且更新每次申请的块数

如果一块都无法提供，则把剩余的内存挂到自由链表，然后向系统heap申请空间，如果申请失败，则看看自由链表还有没有可用的块，如果也没有，则最后调用一级空间配置器



### **空间配置函数allocate**

首先先要检查申请空间的大小，如果大于128字节就调用第一级配置器，小于128字节就检查对应的空闲链表，如果该空闲链表中有可用数据块，则直接拿来用（拿取空闲链表中的第一个可用数据块，然后把该空闲链表的地址设置为该数据块指向的下一个地址），如果没有可用数据块，则调用refill重新填充空间。

### 空间释放函数deallocate

首先先要检查释放数据块的大小，如果大于128字节就调用第一级配置器，小于128字节则根据数据块的大小来判断回收后的空间会被插入到哪个空闲链表。

### **重新填充空闲链表refill**

在用allocate配置空间时，如果空闲链表中没有可用数据块，就会调用refill来重新填充空间，新的空间取自内存池。缺省取20个数据块，如果内存池空间不足，那么能取多少个节点就取多少个。

从内存池取空间给空闲链表用是chunk_alloc的工作，首先根据end_free-start_free来判断内存池中的剩余空间是否足以调出nobjs个大小为size的数据块出去，如果内存连一个数据块的空间都无法供应，需要用malloc取堆中申请内存。

假如山穷水尽，整个系统的堆空间都不够用了，malloc失败，那么chunk_alloc会从空闲链表中找是否有大的数据块，然后将该数据块的空间分给内存池（这个数据块会从链表中去除）。

### 二级内存池

二级内存池采用了16个空闲链表，这里的16个空闲链表分别管理大小为8、16、24......120、128的数据块。这里空闲链表节点的设计十分巧妙，这里用了一个联合体既可以表示下一个空闲数据块（存在于空闲链表中）的地址，也可以表示已经被用户使用的数据块（不存在空闲链表中）的地址。

 

<img src="file:///C:\Users\COSTCO~1\AppData\Local\Temp\msohtmlclip1\02\clip_image002.png" alt="img" style="zoom:67%;" />



### 总结

1. 使用allocate向内存池请求size大小的内存空间，如果需要请求的内存大小大于128bytes，直接使用malloc。

2. 如果需要的内存大小小于128bytes，allocate根据size找到最适合的自由链表。

   a. 如果链表不为空，返回第一个node，链表头改为第二个node。

   b. 如果链表为空，使用blockAlloc请求分配node。

   ​	x. 如果内存池中有大于一个node的空间，分配竟可能多的node(但是最多20个)，将一个node返回，其他的node添加到链表中。

   ​	y. 如果内存池只有一个node的空间，直接返回给用户。

   ​	z. 若果如果连一个node都没有，再次向操作系统请求分配内存。

   ​			①分配成功，再次进行b过程。

   ​			②分配失败，循环各个自由链表，寻找空间。

   ​				I. 找到空间，再次进行过程b。

   ​				II. 找不到空间，抛出异常。

3. 用户调用deallocate释放内存空间，如果要求释放的内存空间大于128bytes，直接调用free。

4. 否则按照其大小找到合适的自由链表，并将其插入。

## **迭代器iterator**

  提供一种方法，能够依序访问容器内所含的元素。迭代器是一种类似指针的行为。

  迭代器最重要的实现就是对operator*和operator->进行重载工作。要实现迭代器，必须要对容器的实现细节有非常丰富的理解，因此STL容器都提供有专属的迭代器。

### 获取迭代器类型

  算法中必须要声明有一个变量，以“迭代器所指对象的型别”为型别。通过使用function template的参数推导(argument deduction)机制实现。

**iterator-traits**

<img src="C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200625220939669.png" alt="image-20200625220939669" style="zoom:67%;" />

最常用到的迭代器相应型别有五种：value type，difference type，pointer，reference，iterator category。如果希望所开发的容器与STL结合，一定要为容器的迭代器定义五种型别。“特性萃取机”traits会很忠实地将原汁原味地榨取出来。

```C++
template <class I>
struct iterator_traits {
    typedef typename I::iterator_category  iterator_category;
    typedef typename I::value_type         value_type;
    typedef typename I::difference_type    difference_type;
    typedef typename I::pointer            pointer;
    typedef typename I::reference          reference;
};
```

### 迭代器型别之一：value type

**value_type:** 指迭代器所指向对象的型别。任何一个打算与STL算法有完美搭配的class，都应该定义自己的value type内嵌型别。

### 迭代器型别之二：difference type

**difference_type：**表示两个迭代器之间的距离，因此也可以用来表示一个容器的最大容量。如果一个泛型算法提供计数的功能，例如count()，其传回的值就必须使用迭代器的difference_type。

```C++
template <class I，class T>
typename iterator_ traits<I>: : difference_type // 这一整行是函数回返型别
count(I first, I last， const T& value) {
    typename iterator_ traits<I>: : difference_type n = 0;
    for (; first != last; ++first)
        if (*first == value)
            ++n;
    return n;
}	
```

针对相应型别difference type, traits 的如下两个( 针对原生指针而写的)特化版本，以C++内建的ptrdiff_ t (定义于<cstddef\>头文件)作为原生指针的difference type:

```c++
template <class I>
struct iterator_ traits {
        …
        typedef typename I::difference_ type  difference_ type;
};
//针对原生指针而设计的 “偏特化( partial specialization)” 版
template <class T>
struct iterator_traits<T*> {
        …
        typedef ptrdiff_t difference_ type;
};
//针对原生的pointer-to-const而设计的“ 偏特化(partial specialization)”版
template <class T>
struct iterator_ traits<const T*> {
        …
        typedef ptrdiff_t difference_ type;
};
```

现在，任何时候当需要任何迭代器I的difference type，可以写：

typename iterator_traits<I>::difference_ type;

### 迭代器型别之三：reference type

**reference** **type:** 在C++中，函数如果要传回左值，都是以by reference的方式进行，所以当是个mutable iterators时,如果其value type是T, 那么*p的型别不应该是T,应该是T&。将此道理扩充，如果p是一个constant iterators,其value type 是T,那么*p的型别不应该是const T，而应该是const T&。这里所讨论的*p的型别，即所谓的reference type.。

### 迭代器型别之四：pointer type

**pointer type:** pointers和references在C++中有非常密切的关联。如果“ 传回一个左值,令它代表p所指之物”是可能的，那么“传回一个左值， 令它代表P所指之物的地址”也一定可以。也就是说，我们能够传回一个pointer, 指向迭代器所指之物。

这些相应型别已在ListIter class中出现过:

```c++
Item& operator* () const { return *ptr; }
Item* operator->() const { return ptr; }
```

Item& 便是ListIter class的reference type， Item* 便是其pointer type.

```C++
template <class I>
struct iterator_ traits {
        …
        typedef typename I::pointer  pointer;
        typedef typename I::reference reference;
};
//针对原生指针而设计的 “偏特化( partial specialization)” 版
template <class T>
struct iterator_traits<T*> {
        …
        typedef T* pointer;
        typedef T& reference;
};
//针对原生的pointer-to-const而设计的“ 偏特化(partial specialization)”版
template <class T>
struct iterator_ traits<const T*> {
        …
        typedef const T* pointer;
        typedef const T& reference;
};
```

### 迭代器型别之五：iterator category

**iterator_category:** 迭代器类型。输入、输出、前向、双向、随机。

## **容器Container**

### 序列式容器sequential container

#### vector

​    与array相似，唯一差别在于空间的灵活运用。array是静态空间，一旦配置了就不改变；vector是动态空间，随着元素的加入，内部机制会自行扩充空间以容纳新元素。

​    vector的实现技术，关键在于其对大小的控制以及重新配置时的数据移动效率。

![image-20200625214533651](C:\Users\Costco424\AppData\Roaming\Typora\typora-user-images\image-20200625214533651.png)

#### list

#### deque

#### stack

#### queue

#### heap

#### priority_queue



### 关联式容器associative contaier

#### RB-tree

#### set

#### map

#### multimap

#### multiset

#### hashtable

#### hash_set

#### hash_map

## 算法 algorithms



## **仿函数functors/ 函数对象function table**

  就实现意义而言，“函数对象”比较贴切:一种具有函数特质的对象。不过，就其行为而言，以及就中文用词的清晰漂亮与独特性而言，“仿函数 ”一词比较突出。因此，本书绝大部分时候采用“ 仿函数”一词。这种东西在调用者可以像函数一样地被调用(调用)，在被调用者则以对象所定义的function call operator 扮演函数的实质角色。

  就实现观点而言，仿函数其实就是一个“行为类似函数”的对象。为了能够“行为类似函数”，其类别定义中必须定义（或说改写、重载）function call运算子(operator())。

仿函数的主要用途是搭配STL算法。

## 配接器adapters

  配接器(adapters) 在STL组件的灵活组合运用功能上，扮演着轴承、转换器的角色。Adapter 这个概念，事实上是一种设计模式(design pattern) 。

分类：

### **应用于容器 container adapters**

queue、stack

### 应用于迭代器 iterator adapters

insert iterators、reverse iterators、iostream iterators. 可由<iterator\>获得

### 应用于仿函数functor adapters