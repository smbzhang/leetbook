## HashMap无序Map

### C++的hash\_map & unordered\_map

这两个的内部结构都是使用哈希表来实现的，区别是 unordered\_map 在 C++11中被引入标准库了。

### 特性

1. 关联性：通过key去检索value，而不是通过绝对地址（和顺序容器不同）

2. 无序性：使用hash表存储，内部无序

3. Map : 每个值对应一个键值

4. 键唯一性：不存在两个元素的键一样

5. 动态内存管理：使用内存管理模型来动态管理所需要的内存空间

### hash\_table 与 Bucket

unorderedmap的内部是使用 hashtable 数据结构存储的，hash就会存在冲突，关于怎么处理冲突可以去看一下STL源码剖析这本书。

当出现冲突的时候会通过链表连在前一个元素后面，这个位置被称为bucket

![](/images/hashmap.png)

所以unordered\_map内部其实是由很多哈希桶组成的，每个哈希桶中可能没有元素，也可能有多个元素。

### 迭代器

unordered\_map的迭代器是一个指针，指向这个元素，通过迭代器来取得它的值。

```
unordered_map<Key,T>::iterator it;
(*it).first;             // the key value (of type Key)
(*it).second;            // the mapped value (of type T)
(*it);                   // the "element value" (of type pair<const Key,T>)
```

它的键值分别是迭代器的first和second属性。

```
it->first;               // same as (*it).first   (the key value)
it->second;              // same as (*it).second  (the mapped value)
```

### 构造方式

```
- 构造空的容器
- 复制构造
- 范围构造
- 用数组构造

typedef unordered_map<string,string> stringmap;
int main ()
{
  stringmap first;                                                 // 空
  stringmap second ( {{"apple","red"},{"lemon","yellow"}} );       // 用数组初始
  stringmap fourth (second);                                       // 复制初始化
  stringmap sixth (second.begin(),second.end());                   // 范围初始化
  for (auto& x: sixth) cout << " " << x.first << ":" << x.second;
  cout << endl;
  return 0;
}
```

### 容量操作

#### empty

```
bool empty() const noexcept;
```

#### size

```
size_type size() const noexcept;
```



