## 题目描述（简单难度）
![](/assets/021-1.png)
合并两个排序链表

## 思路
这里我们可以循环遍历两个链表，在循环比较中按照大小顺序把结点存入一个queue中，然后链接队列中的结点返回，这样的话就会额外使用一个队列的空间，还要遍历两边。下面介绍不使用额外空间并只遍历一遍的解法。
### 思路一
![](/assets/021-2.png)
定义两个指针，head和pcur，head指向返回的链表头。
首先要确定head和pcur的指向。分四种情况：
- case1： 当l1空l2非空
- case2： 当l1空l2空
- case3： 当l1非空l2空
- case4： 当l1非空l2非空

在确定head的时候比较复杂。然后遍历两个链表，直到一个链表为空，pcur->next指向非空的链表。这里注意，pcur可能为空，如果其实链表有一个为空，pcur循环完之后就为空，这个需要特殊处理。

### 思路二
![](/assets/021-3.png)

上面思路一的边界情况比较多，很容易出错，这里我们采用带头结点的链表简化边界判断。

创建一个结点 head，里面存放的是 INT_MIN，这样就不用去确定head的指向问题，并且不会出现循环结束，pcur为空的情况。

## 代码
### 解法一

```
// 解法一， 头指针的确定很繁琐，条件很多
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode *head = NULL, *pcur = NULL;
    while(l1 && l2) {
        if (pcur == NULL) {
            pcur = (l1->val <= l2->val) ? l1 : l2;
            head = pcur;
            if (l1->val <= l2->val) l1 = l1->next;
            else l2 = l2->next;
            continue;
        }
        if (l1->val <= l2->val) {
            pcur->next = l1;
            l1 = l1->next;
            pcur = pcur->next;
        }else{
            pcur->next = l2;
            l2 = l2->next;
            pcur = pcur->next;
        }
    }
    if (pcur == NULL) head = l1 ? l1 : l2;
    else pcur->next = l1 ? l1 : l2;
    return head;
}
```

### 解法二
```
// 解法二，使用类似带头结点的链表来实现，头结点存放 INT_MIN，这样返回的时候只需要返回头结点的next，不需要判断第一个结点是l1还是l2
ListNode* mergeTwoLists_2(ListNode* l1, ListNode* l2) {
    ListNode *head = new ListNode(INT_MIN), *pcur = head;
    while (l1 && l2) {
        if (l1->val <= l2->val) {
            pcur->next = l1;
            l1 = l1->next;
            pcur= pcur->next;
        }else{
            pcur->next = l2;
            l2 = l2->next;
            pcur= pcur->next;
        }
    }
    pcur->next = l1 ? l1 : l2;
    return head->next;
}
```

**时间复杂度：** O(n)
**空间复杂度：** O(1)

## 总结
链表的问题，当头结点边界条件很多的时候，可以创建一个带头结点的链表，简化边界处理过程