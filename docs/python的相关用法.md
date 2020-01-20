# python 中的用法笔记

## python 的数据结构

### python 中的值类型和引用类型的数据结构以及深浅拷贝

#### 值类型
在Python中，数值，字符串，元组本身是值类型，本身不允许被修改。

例子：
```
a = 1
b = a
a = 2
print(b)  #输出的结果是1
```
修改值类型的值，只是让它指向一个新的内存地址，并不会改变变量b的值

#### 引用类型

在Python中，列表，集合，字典是引用类型，本身允许修改。

```
list_a = [1,2]
list_b = list_a
list_a[0] = 3
print(list_b)  #此时的输出结果是[3,2]
```

修改引用类型的值，因为listb的地址和lista的一致，所以也会被修改

一般只为了复制值，可以使用分片操作

list_b = list_a[:]



## python 中的巧妙用法

### join 的妙用
```
words = ["ss","sssd"]
path = '/'.join(words)  // path=ss/sssd
```