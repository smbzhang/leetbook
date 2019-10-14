## 题目描述（困难难度）
https://leetcode.com/problems/merge-k-sorted-lists/
![](/assets/023-1.jpg)

合并 k 个拍序好的单链表。

## 思路

### 思路一
可以使用递归来实现，之前做过merge两个拍序好的链表，请参考 [021-MergeTwoSortedLists](021-MergeTwoSortedLists.md)

这里我们可以使用递归实现，取出第一个链表，递归计算剩余 k-1 个链表的merge结果，然后按照合并两个链表的解法合并第一个和 2至k个链表的合并结果。递归结束的条件是lists的个数 < 2; 具体实现看代码。

### 思路二
直接遍历一遍K个链表的所有结点，并且进行判断和链接，最后返回结果。



时间复杂度 O(nk)，(n 是最长链表的长度) 空间复杂度O(1)

### 思路三
思路二中，查找一个最小值需要 O(K)的时间，使用堆结构存储可以达到时间复杂度 O(lng(k));

使用优先队列存储链表，比较的规则是多有链表的head的val大小。

![](/assets/023-2.jpg)
存储和插入的时间复杂度都是 log(k)

总的时间复杂度 N(log(k)), (N 是所有的结点个数)


## 代码
### C++实现
#### 思路一

```
// 使用递归实现
ListNode* mergeKLists_2(vector<ListNode *>& lists) {
    ListNode *ret = merge(lists, 0);
    return ret;
}
ListNode* merge(vector<ListNode *> &lists, int pos) {
    if (pos == lists.size()) return NULL;
    if (pos == lists.size() - 1) return lists[pos];
    ListNode *cur = new ListNode(0);
    ListNode *ret = cur;
    ListNode *list1 = lists[pos];
    ListNode *list2 = merge(lists, pos+1);
    while (list1 || list2) {
        if (!list1) {
            cur->next = list2;
            break;
        }
        if (!list2) {
            cur->next = list1;
            break;
        }
        if (list1->val <= list2->val) {
            cur->next = list1;
            list1 = list1->next;
            cur = cur->next;
        }else{
            cur->next = list2;
            list2 = list2->next;
            cur = cur->next;
        }
    }
    return ret->next;
}
```

#### 思路二
```
ListNode* mergeKLists(vector<ListNode*>& lists) {
    ListNode *head = new ListNode(0);
    ListNode *ret = head;
    while(1) {
        int min = INT_MAX;
        ListNode *cur = NULL;
        int pos = -1;
        for (int i = 0; i < lists.size(); i++) {
            if (lists[i] != NULL) {
                if (lists[i]->val < min) {cur = lists[i]; min = lists[i]->val; pos = i;}
            }
        }
        if (!cur) break;
        lists[pos] = lists[pos]->next;
        head->next = cur;
        head = head->next;
    }
    return ret->next;
}
```

**时间复杂度：** O(nk)
**空间复杂度：** O(1) 

#### 思路三
```
// 使用优先队列实现
class cmp {
public:
    bool operator()(const ListNode *l1, const ListNode *l2) {
        return l1->val > l2->val;
    }
};
ListNode *mergeKLists_3(vector<ListNode *>& lists) {
    std::priority_queue<ListNode *, vector<ListNode *>, cmp> myqueue;
    ListNode *cur = new ListNode(0), *ret = cur;
    for (int i = 0; i < lists.size(); i++) {
        if (lists[i] != NULL) myqueue.push(lists[i]);
    }
    while (!myqueue.empty()) {
        ListNode * node = myqueue.top();
        cur->next = node;
        cur = cur->next;
        node = node->next;
        myqueue.pop();
        if (node != NULL) {
            myqueue.push(node);
        }
    }
    return ret->next;
}
```

**时间复杂度：** O(Nlog(k))
**空间复杂度：** O(1) 

## 总结
除了常规的递归和遍历之外，使用优先队列处理 K 排序问题，会节省很多时间。