# HashMap无序Map

## C++的hash\_map & unordered\_map

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

### 元素操作

#### find

```
iterator find ( const key_type& k );
```

查找key所在的元素。

* 找到：返回元素的迭代器。通过迭代器的second属性获取值

* 没找到：返回unordered\_map::end

#### insert

插入有几种方式：

* 复制插入 \(复制一个已有的pair的内容\)
* 数组插入（直接插入一个二维数组）
* 范围插入（复制一个起始迭代器和终止迭代器中间的内容）
* 数组访问模式插入\(和数组的\[\]操作很相似\)
  具体看后面的例子

#### at

```
mapped_type& at ( const key_type& k );
```

查找key所对应的值

* 如果存在：返回key对应的值，可以直接修改，和\[\]操作一样。
* 如果不存在：抛出 out\_of\_range 异常.

#### erase

擦除元素也有几种方式：

```
1. 通过位置（迭代器）
iterator erase ( const_iterator position );
2. 通过key
size_type erase ( const key_type& k );
3. 通过范围（两个迭代器）
iterator erase ( const_iterator first, const_iterator last );
```

#### clear

```
void clear() noexcept
```

清空unordered\_map

#### swap

```
void swap ( unordered_map& ump );
```

交换两个unordered\_map（注意，不是交换特定元素，是整个交换两个map中的所有元素）

### 示例代码

```
// unordered_map::insert
#include <iostream>
#include <string>
#include <unordered_map>
using namespace std;

void display(unordered_map<string,double> myrecipe,string str)
{
    cout << str << endl;
    for (auto& x: myrecipe)
        cout << x.first << ": " << x.second << endl;
    cout << endl;
}

int main ()
{
    unordered_map<string,double>
    myrecipe,
    mypantry = {{"milk",2.0},{"flour",1.5}};

    /****************插入*****************/
    pair<string,double> myshopping ("baking powder",0.3);
    myrecipe.insert (myshopping);                        // 复制插入
    myrecipe.insert (make_pair<string,double>("eggs",6.0)); // 移动插入
    myrecipe.insert (mypantry.begin(), mypantry.end());  // 范围插入
    myrecipe.insert ({{"sugar",0.8},{"salt",0.1}});    // 初始化数组插入(可以用二维一次插入多个元素，也可以用一维插入一个元素)
    myrecipe["coffee"] = 10.0;  //数组形式插入

    display(myrecipe,"myrecipe contains:");

    /****************查找*****************/
    unordered_map<string,double>::const_iterator got = myrecipe.find ("coffee");

    if ( got == myrecipe.end() )
        cout << "not found";
    else
        cout << "found "<<got->first << " is " << got->second<<"\n\n";
    /****************修改*****************/
    myrecipe.at("coffee") = 9.0;
    myrecipe["milk"] = 3.0;
    display(myrecipe,"After modify myrecipe contains:");

    /****************擦除*****************/
    myrecipe.erase(myrecipe.begin());  //通过位置
    myrecipe.erase("milk");    //通过key
    display(myrecipe,"After erase myrecipe contains:");

    /****************交换*****************/
    myrecipe.swap(mypantry);
    display(myrecipe,"After swap with mypantry, myrecipe contains:");

    /****************清空*****************/
    myrecipe.clear();
    display(myrecipe,"After clear, myrecipe contains:");
    return 0;
}
```



