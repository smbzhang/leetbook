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

下面两个其实实现是一样的，只不过第一个没有使用 num1和num2来进行设计，所以写出来的程序很丑陋。

```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *pcur1 = l1, *pcur2 = l2, *pcur = NULL, *ret = NULL;
        int carry = 0;
        while (pcur1 != NULL) {
            int num1 = pcur1->val;
            if (pcur2 != NULL) {
                int num2 = pcur2->val;
                    int mod = (num1 + num2 + carry) % 10;
                    if (pcur == NULL) {
                        pcur = new ListNode(mod);
                        ret = pcur;
                    }else {
                        ListNode *node = new ListNode(mod);
                        pcur->next = node;
                        pcur = node;
                    }
                    carry = (num1 + num2 + carry) / 10;
                    pcur2 = pcur2->next;
            }else
                break;
            pcur1 = pcur1->next;
        }
        while(pcur1 != NULL) {
            int mod = (pcur1->val + carry) % 10;
            carry = (pcur1->val + carry) / 10;
            ListNode *node = new ListNode(mod);
            pcur->next = node;
            pcur = node;
            pcur1 = pcur1->next;
        }
        while(pcur2 != NULL) {
            int mod = (pcur2->val + carry) % 10;
            carry = (pcur2->val + carry) / 10;
            ListNode *node = new ListNode(mod);
            pcur->next = node;
            pcur = node;
            pcur2 = pcur2->next;

        }

        if (carry != 0) {
            ListNode *node = new ListNode(carry);
            pcur->next = node;
            pcur = node;
        }
        return ret;
    }
};
```

```
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode *ret = NULL, *pcur = NULL;
        int num1 = 0, num2 = 0, carry = 0;
        while (l1 != NULL || l2 != NULL) {
            num1 = (l1 == NULL) ? 0 : l1->val;
            num2 = (l2 == NULL) ? 0 : l2->val;
            int mod = (num1 + num2 + carry) % 10;
            carry = (num1 + num2 + carry) / 10;
            ListNode *node = new ListNode(mod);
            if (pcur == NULL) {
                pcur = node;
                ret = pcur;
            } else {
                pcur->next = node;
                pcur = node;
            }
            if (l1 != NULL) l1 = l1->next;
            if (l2 != NULL) l2 = l2->next;
        }
        if (carry != 0) {
            ListNode *node = new ListNode(carry);
            pcur->next = node;
        }
        return ret;
    }
};
```



