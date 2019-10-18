## 题目描述（困难难度）
https://leetcode.com/problems/reverse-nodes-in-k-group/
![](/assets/025-1.jpg)

给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

你的算法只能使用常数的额外空间。

你不能只是单纯的改变节点内部的值，而是需要实际的进行节点交换。

## 思路

其实这个题就是一个链表反转的题。我们把输入的链表分组，一组k个结点，依次反转并且链接，返回最终的结果。

关于单链表的反转，可以使用递归来实现，还可以使用头插法来实现。

## 代码

### C++ 实现

```
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        int n = 0;
        ListNode *cur = head;
        while(cur) {n++; cur = cur->next;}
        if (n < k || k <= 1) return head;
        cur = head;
        for (int i = 0; i < k; i++) {
            cur = cur->next;
        }
        auto ret = reverseList(head, k);
        head->next = reverseKGroup(cur, k);
        return ret;
    }

    // 使用递归实现单链表的反转
    ListNode *reverseList(ListNode* head, int k) {
        if (k <= 1 || head == NULL || head->next == NULL) {
            return head;
        }
        ListNode *tail = reverseList(head->next, k - 1);
        head->next->next = head;
        head->next = NULL;
        return tail;
    }
    // 使用头插法实现 O(n) 非递归的单链表反转，找到 head，然后遍历中的每一个元素都插入到head的前面。
};
```

上面使用了递归实现单链表的反转，其实可以使用O(n)的头插法来实现链表的反转。

