## 题目描述（中等难度）
https://leetcode.com/problems/swap-nodes-in-pairs/
![](/assets/024-1.png)

反转链表相邻元素

## 思路

### 思路一

递归求解, 单独处理前两个结点，后面的结点进行反转并且返回头结点。
![](/assets/024-2.jpg)

### 思路二

遍历一遍，直接反转完成。为了处理方便新建一个头指针结点空的。
![](/assets/024-3.jpg)


## 代码
### C++实现
#### 思路一

```
class Solution {
public:
    // 递归实现
    ListNode* swapPairs_2(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode *ret = swapPairs_2(head->next->next);
        auto node = head->next;
        head->next = ret;
        node->next = head;
        return node;
    }
};
```

### 思路二
```
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode *n = new ListNode(0), *cur = n;
        n->next = head;
        while(cur->next && cur->next->next) {
            auto node1 = cur->next;
            auto node2 = cur->next->next;
            cur->next = node2;
            node1->next = node2->next;
            node2->next = node1;
            cur = node1;
        }
        return n->next;
    }
};
```

## 总结

妙用头指针结点，可以免去单独处理head结点麻烦的逻辑。代码更清爽。