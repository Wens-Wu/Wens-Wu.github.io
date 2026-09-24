---
title: Hello Python
date: 2026-09-24 16:36:51
tags:
---

教材：https://github.com/jackfrued/Python-100-Days/tree/master

---

### Basics

1.标识符,关键字.

2.import, from...import.

3.运算符

|            运算符             |         描述         |
| :---------------------------: | :------------------: |
|           [] 、 [:]           |      索引、切片      |
|               ~               |   按位取反（-x-1）   |
|   **  、 * 、 / 、 % 、 //    | 幂、乘、除、模、整除 |
|           >> 、 <<            |      右移、左移      |
|         is 、 is not          |      身份运算符      |
|         in 、 not in          |      成员运算符      |
|       not 、 or 、 and        |      逻辑运算符      |
| **= 、 &= 、 ^= 、 >>= 、 <<= |      赋值运算符      |

其中，切片 [:] 的取值是左闭右开；

```python
b=[0,1,2,3,4,5,6]
print(b[2:5])
# 输出[2, 3, 4]
```

4.结构

- 分支结构

  - if-else

  - match-case

    ```python
    status = 100
    match status:
        case 90:
           print("太轻了")
        case 100:
            print("刚好")
    ```

    

- 循环结构
  - for in循环
  - while循环
  - break-continue

### 标准数据类型

> 变量是数据的载体，简单的说就是一块用来保存数据的内存空间。

1.number(int, float,bool,complex),str（不可变类型，不能直接修改某一个字符）,bool.

Python3中只有一种整数类型int，表示为长整型。

2.list：由一系元素按特定顺序构成的数据序列。

操作列表：list.remove(), list.pop(), list.clear(), list.index("element",1), list.count("element"), list.sort(), list.reverse().

索引，切片运算，元素遍历，嵌套列表。

3.tuple：多个元素按照一定顺序构成的序列。tuple和list最核心的区别就是：元组的元素不能修改。

4.set：是一种无序、可变的数据类型，用于存储唯一的元素。集合中的元素不会重复，并且可以进行交集、并集、差集等常见的集合操作。

5.dict：字典是一种映射类型，用 {} 标识，它是一个键(key) : 值(value)的集合。

```python
item_list = [1, 2, 3, 4, 5]   # list[]
item_tuple=(1,2,3,4,5) # tuple()
item_set = {1, 2, 3, 4, 5}  # set{}
item_str = "1,2,3,4,5" #str""
```

|     list     |    tuple     |      set       |   str    |
| :----------: | :----------: | :------------: | :------: |
|     有序     |     有序     |      无序      |   有序   |
| 允许重复元素 |              | 不允许重复元素 |   允许   |
| 支持索引访问 | 支持索引访问 | 不支持索引访问 | 支持索引 |

6.bytes
