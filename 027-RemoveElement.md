## 题目描述（简单难度）
https://leetcode.com/problems/remove-element/
![](/assets/027-1.jpg)

给一个数组，让找出所有不等于val值的个数返回。并且将非val值的元素放到数组的最前面元素中。

## 思路

这个题和 [026-RemoveDuplicatesFromSortedArray](026-RemoveDuplicatesFromSortedArray.md)是类似的，如出一辙，这里我们依然采用快慢指针实现，pos用于指向前k个非val的数，pos从0到k，i则是遍历nums数组的快指针没遇到val值就直接前进。遇到非val的值就设置 nums[pos++] = nums[i]；i继续前进直到nums.size();

在上面这个循环中定义一个整数ret记录非 val 值的个数。用于返回。

这样我们没有使用额外的数组来存储中间结果，在原来数组上面直接修改，边可以满足要求。

总的时间复杂度是 O(n))

## 代码
### C++实现

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int pos = 0, ret = 0;
        for (int i = 0; i < nums.size(); i++) {
            while (i < nums.size() && nums[i] == val) i++;
            if (i >= nums.size()) break;
            nums[pos++] = nums[i];
            ret++;
        }
        return ret;
    }
};
```

**时间复杂度：** O(n)
**空间复杂度：** O(1) 


## 总结

数组链表问题，在进行求解的时候，如果题目要求不使用额外的数组空间，空间复杂度比较苛刻O(1),那么就可以考虑快慢指针来实现。