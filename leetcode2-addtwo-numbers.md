## 题目描述（中等难度）![](/assets/import.png)思路

## ![](/assets/list.png)

这个题就是一个大数加法题，两个链表表示两个大数字，不过链表的节点是按照数字的低位到高位进行排列的，所以我们从头遍历两个链表并把结点值相加，进位用carry表示。伪代码如下：

```
1. 初始化两个NULL节点 pcur 和 ret， ret用来返回结果链表，pcur指向当前操作的结果链表节点
2. 初始化一个int类型的进位值 carry = 0
3. 初始化两个int类型的值，表示当前两个链表的当前元素的值， num1 = 0 num2 = 0
4. while(l1 != NULL || l2 != NULL)
    1. num1 = (l1 == NULL) ? 0 : l1->val
    2. num2 = (l2 == NULL) ? 0 : l2->val
    3. 计算 mod = num1 + num2 + carry % 10
    4. 计算 carry = num1 + num2 + carry / 10
    如果 pcur 等于 NULL
        5. 新建一个 node 节点，使用 mod 初始化， 并且 pcur 指向这个节点， ret = pcur 初始化返回列表头
    否则的话
        5. 新建一个 node 节点，使用 mod 初始化， 并且 pcur->next 指向这个节点, pcur = node
    6. 判断 l1 和 l2 是否为 NULL 并向前移动一个节点
5. 判断 carry 是不是0，如果不是的话，创建一个节点node使用carry初始化，pcur指向这个node
6. 返回 ret
```

### 代码

#### C++实现

```

```



