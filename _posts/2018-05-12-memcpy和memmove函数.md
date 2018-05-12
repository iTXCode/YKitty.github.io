---
layout:     post
title:      memcpy函数与memmove函数
subtitle:   相同与不同
date:       2018-05-10
author:     YKitty
header-img: img/内存拷贝函数/300694.jpg
catalog: true
tags:
    -内存
    -字符串
---

# memcpy和memmove函数



---

## [memcpy函数](https://baike.baidu.com/item/memcpy/659918?fr=aladdin)

---

###**源函数**

```c
void *memcpy(void *dest, const void *src, size_t n 
```

###**作用**

(小编自己的理解)内存拷贝。从src内存空间移动n个元素，将其赋值给dest的内存中，从内存中第一个地址所指向的数据开始赋值，直到赋值n个数据，该函数调用结束，同时返回dest，即返回被赋值的内存的起始位置。返回dest的意义是可是增加链接属性。

**memcpy函数实现**:**(my_memcpy)**原谅小编没有找到源代码

```c
#include <assert.h> //包含assert函数的头文件
#define my_type int //自己定义要进行内存赋值的数据类型

my_type *my_memcpy(my_table *dest, const my_type *src, size_t n)
{
    my_type *start = dest; //用于返回dest的初始地址,使该函数具有链接属性
    assert(dest != NULL); //断言dest与src不是空地址
    assert(src != NULL);    
    while (n--)
    {
        *dest++ = *src++;
    }
    return start;
}
```

不了解assert函数的同学们，可以戳一戳[assert](https://baike.baidu.com/item/assert/10931289?fr=aladdin)了解一下哦！

---

## memmove函数

---

###源函数

```c
void *memmove(void *dest, const void *src, size_t n)
```

###作用

(小编自己的理解)内存拷贝。基本功能与memcpy函数一致。不一样的地方在于，若是目标区域与源区域有重叠的部分的话，可以在源区域未被目标区域覆盖的时候，将其拷贝到目标区域当中。从而实现内存拷贝的功能。

**memmove函数的实现**:**(my_memmove)**原谅小编没有找到源代码

```c
#include <assert.h> //包含assert函数的头文件
#define my_type int //自己定义要进行内存赋值的数据类型

my_type *my_memcpy(my_table *dest, const my_type *src, size_t n)
{
    my_type *start = dest; //用于返回dest的初始地址,使该函数具有链接属性
    assert(dest != NULL); //断言dest与src不是空地址
    assert(src != NULL);    
    while (n)
    {
        *(dest + n - 1) = *(src + n -1); //从最后一个要拷贝的内存开始拷贝依次先前拷贝
        n--;
    }
    return start;
}
```

---

## 对比

####1.**memcpy函数与memmove函数进行对比**

- **memcpy函数对于处理内存不重叠的情况，memmove函数可以处理内存重叠的情况**

分情况：

<1> 内存不重叠

![](https://raw.githubusercontent.com/YKitty/YKitty.github.io/master/img/%E5%86%85%E5%AD%98%E6%8B%B7%E8%B4%9D%E5%87%BD%E6%95%B0/%E6%97%A0%E9%87%8D%E5%8F%A0%E6%83%85%E5%86%B5%E6%9C%80%E7%BB%88.png)

<2>内存重叠，但不影响函数和使用

![](https://raw.githubusercontent.com/YKitty/YKitty.github.io/master/img/%E5%86%85%E5%AD%98%E6%8B%B7%E8%B4%9D%E5%87%BD%E6%95%B0/%E9%87%8D%E5%8F%A0%E4%B8%8D%E5%87%BA%E9%94%99.png)

<3>内存重叠只能用memmove函数

![](https://raw.githubusercontent.com/YKitty/YKitty.github.io/master/img/%E5%86%85%E5%AD%98%E6%8B%B7%E8%B4%9D%E5%87%BD%E6%95%B0/%E5%8F%AA%E8%83%BD%E7%94%A8memmove%E5%87%BD%E6%95%B0%E7%9A%84.png)

- **原理区别**

**原理:**memcpy函数是从左边一个一个的将src中的值拷贝到dest中，二memmove函数是从右边开始拷贝的。所以对于memcpy函数只有<1><2>情况可以使用，二对于<3>情况则会出现问题。而memmove函数因为是从右边开始拷贝的就不会出现错误。

**memcpy的错误:**会将第一个和第二个拷贝过去的最后再拷贝到最后的第一个和第二个上。就会出现错误。

#### 2.内存拷贝函数与字符串拷贝函数的对比

- 使用的类型不同，strcpy函数只对字符串进行操作；memmove函数和memcpy函数对所有类型都适用，为内存拷贝
- strcpy函数以’\0’为拷贝的结束条件；而memmove函数和memcpy函数则是以第三个参数num进行控制拷贝



