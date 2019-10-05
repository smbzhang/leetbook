## 题目描述（简单难度）
![](/assets/019-1.png)

只遍历一遍，删除单向链表的倒数第 n 个节点，返回链表头指针

## 思路
抛开限制条件，我们可以先遍历一遍计算出一共有多少节点，然后第二遍遍历删除指定节点。
如果只遍历一遍的话，可以使用快慢指针的方法，快指针领先慢指针n个结点，然后同时遍历快慢指针，当快指针到达尾部的时候，慢指针指向的就是倒数第 n + 1个节点
![](/assets/019-2.png)

## 代码
### C++ 实现
```
// 采用快慢指针的解法
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        if (head == NULL || n == 0) return head;
        ListNode *slow = head, *fast = head;
        for (int i = 0; i < n; i++) {
            if (fast != NULL)
                fast = fast->next;
            else
                // 这一步其实没必要做，因为题目给的都是有效的
                return head;
        }
        // 删除 头 的情况，如果fast最终指向 null ，说明要删除头指针
        if (!fast) return head->next;
        while(fast->next) {
            slow = slow->next;
            fast = fast->next;
        }
        slow->next = slow->next->next;
        return head;
    }
};
```
**时间复杂度：** O(n)
**空间复杂度：** O(1) 

# 总结
快慢指针在链表中的使用


