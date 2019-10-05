# HashMap无序Map
{% raw %}

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

![](./images/hashmap.png)

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
{% raw %}
typedef unordered_map<string,string> stringmap;
int main ()
{
  stringmap first;                                                 // 空
  stringmap second ({{"apple","red"},{"lemon","yellow"}});        // 用数组初始
  stringmap fourth (second);                                       // 复制初始化
  stringmap sixth (second.begin(),second.end());                   // 范围初始化
  for (auto& x: sixth) cout << " " << x.first << ":" << x.second;
  cout << endl;
  return 0;
}
{% endraw %}
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

## Go的hashmap

Go语言的map数据结构就是hashmap

### 特性

Go语言的map底层的实现和C++的实现原理相同，同样是基于bucket来实现内存管理的。Go语言中的map和slice以及channel一样是引用类型。键必须是支持相等运算符 \("=="、"!="\) 类型， 如 number、string、 pointer、array、struct，以及对应的 interface。值可以是任意类型，没有限制。slice，map，function都不能当做key。

### 构造方式

```
1. 直接构造
var m1 map[string]float32 = map[string]float32{"C": 5, "Go": 4.5, "Python": 4.5, "C++": 2}
2. make构造
// 创建了一个键类型为string,值类型为int的map
m1 := make(map[string]int)
// 也可以选择是否在创建时指定该map的初始存储能力，如创建了一个初始存储能力为5的map
m2 := make(map[string]int, 5)
```

### 元素操作

```
m := map[string]string{"key0": "value0", "key1": "value1"}
fmt.Printf("map m : %v\n", m)
//map插入
m["key2"] = "value2"
fmt.Printf("inserted map m : %v\n", m)
//map修改
m["key0"] = "hello world!"
fmt.Printf("updated map m : %v\n", m)
```

### 容量

获取键值对的数量 builtin.len

```
len := len(m)
// cap 无效，error
// cap := cap(m)    //invalid argument m (type map[string]string) for cap
// fmt.Printf("map's cap is %v\n", cap)
```

map的容量只能通过len来进行计算，不能使用cap计算容量

### 查找

```
val, ok := m["key0"]
if ok {
    fmt.Printf("map's key0 is %v\n", val)
}
```

通过返回值来进行查找，返回的ok如果是true查找成功，false查找失败

### 删除（如果key不存在不会报错）

delete

```
if val, ok = m["key1"]; ok {
    delete(m, "key1")
    fmt.Printf("deleted key1 map m : %v\n", m)
}
```

因为key 不存在的时候删除操作不会报错所以需要先进行存在判断

### 遍历

```
for k, v := range m {
    fmt.Printf("key -> value : %v -> %v\n", k, v)
}
```
### 注意坑
<font color="red">
从 map 中取回的是一个 value 临时复制品，对其成员的修改是没有任何意义的。
</font>

```
func main() {
    m := map[int]string{1: "x", 2: "w"}
    fmt.Println(m)
    for k, v := range m {
        m[k] = v + v   //修改map的值
        v = v + "copy" //临时复制品，修改无效
    }
    fmt.Println(m)
}
```
<font color="red">
容器和结构体（map and struct）
</font>
```
语法比较：
map[type]struct
map[type]*struct
```

```
func main() {
    type user struct{ name string }
    /*
       当 map 因扩张而重新哈希时，各键值项存储位置都会发生改变。
       因此，map 被设计成 not addressable。
       类似 m[1].name 这种期望透过原 value 指针修改成员的行为自然会被禁 。
    */
    m := map[int]user{ //

        1: {"user1"},
    }
    // m[1].name = "Tom"
    // ./main.go:16:12: cannot assign to struct field m[1].name in map
    fmt.Println(m)

    // 正确做法是完整替换 value 或使用指针。
    u := m[1]
    u.name = "Tom"
    m[1] = u // 替换 value。

    m2 := map[int]*user{
        1: &user{"user1"},
    }

    m2[1].name = "Jack" // 返回的是指针复制品。透过指针修改原对象是允许的。
    fmt.Println(m2)
}
```
<font color="red">
可以在迭代时安全删除键值。但如果期间有新增操作，那么就不知道会有什么意外了。
</font>
```
func main() {
    for i := 0; i < 5; i++ {
        m := map[int]string{
            0: "a", 1: "a", 2: "a", 3: "a", 4: "a",
            5: "a", 6: "a", 7: "a", 8: "a", 9: "a",
        }

        for k := range m {
            m[k+k] = "x"
            delete(m, k)
        }

        fmt.Println(m)
    }
}

输出:
//每次输出都会变化
map[36:x 28:x 32:x 2:x 8:x 10:x 12:x]
map[12:x 6:x 16:x 28:x 4:x 10:x 72:x]
map[12:x 14:x 16:x 18:x 20:x]
map[18:x 10:x 14:x 4:x 6:x 16:x 24:x]
map[12:x 16:x 4:x 40:x 14:x 18:x]
```

## Python的hashmap

同样的python中的字典也是hashmap，实现的方式也是和C++相同。键一般是唯一的，如果重复最后的一个键值对会替换前面的，值不需要唯一。键必须不可变，所以可以用数字，字符串或元组充当，所以用列表就不行。

### 构造方式

```
dict = {'Alice': '2341', 'Beth': '9102', 'Cecil': '3258'}
dict = {}
```

因为python是弱类型语言，所以直接构造方式即可

### 元素访问

如果key值不存在就会抛异常

### 查找

```
dict.has_key(key)
```

有则返回True，否则返回False

### 删除字典元素

```
del dict['Name']  # 删除键是'Name'的条目
dict.clear()      # 清空字典所有条目
del dict          # 删除字典
```

删除不存在的key值，会抛出异常

### 容量

```
len(dict)
```

使用len函数进行容量的计算

### 遍历

```
dict={"a":"Alice","b":"Bruce","J":"Jack"}

# 实例一：
for i in dict:
    print "dict[%s]=" % i,dict[i]

# 实例二：
for i in  dict.items():
    print i

# 实例三：
for (k,v) in  dict.items():
    print "dict[%s]=" % k,v

# 实例四：
for k,v in dict.iteritems():
        print "dict[%s]=" % k,v

# 实例五:
for k in dict.keys():
    print key
```

遍历的方式非常的多
{% endraw %}
