## 题目描述（中等难度）
https://leetcode.com/problems/next-permutation/
![](/assets/031-1.jpg)

返回恰好大于给定的组合的下一个组合。如果给定的组合就是最大的，那么就返回最小的组合


## 思路
### 思路一
找出这个数组的所有不重复排列。然后挨个和给定的哪个序列进行比较，找出恰好大于这个排列的哪个结果。

这个思路的时间复杂度比较高

时间复杂度：O(n!)O(n!)，可能的排列总计有 n!n! 个。

空间复杂度：O(n)O(n)，因为数组将用于存储排列。

### 思路二
转换思路，直接从给定的排列的数组的各个位进行比较操作，找出恰好大于给定组合的结果。那么我们怎么进行比较呢？

整个的思路很难使用语言描述，所以这里贴一下力扣上面的题解的图

https://leetcode-cn.com/problems/next-permutation/solution/xia-yi-ge-pai-lie-by-leetcode/

对于排列中最大的数，只要循环一遍看看nums是不是降序排列的就可以了

![](/assets/031-3.png)

这里我们需要交换a[i - 1] 和恰好大于他的它右边的数，交换完成之后，对它右边的数进行由小到大排序。

排序的过程可以简化，但是，请记住，在从右侧扫描数字时，我们只是继续递减索引直到我们找到 a[i]a[i] 和 a[i-1]a[i−1] 这对数。其中，a[i] > a[i-1]a[i]>a[i−1]。因此，a[i-1]a[i−1] 右边的所有数字都已按降序排序。此外，交换 a[i-1]a[i−1] 和 a[j]a[j] 并未改变该顺序。因此，我们只需要反转 a[i-1]a[i−1] 之后的数字，以获得下一个最小的字典排列。


![](/assets/031-2.gif)

**时间复杂度：** O(n)
**空间复杂度：** O(1) 

## 代码
### C++实现

```
class Solution {
public:
    void nextPermutation(vector<int>& nums) {
        if (nums.size() <= 1) return;
        // 从十位开始向更高位遍历，直到出现高位右边有大于高位数字的
        int n = nums.size(), i = n - 1;
        for (; i > 0; i--) {
            if (nums[i - 1] < nums[i]) break;
        }
        // 如果这个数是最大的那么 返回最小的值
        if (i <= 0) {
            std::sort(nums.begin(), nums.end());
        }else{
            int high = i - 1, low;
            // 找出高位后面恰好大于 高位的那个数
            for (int i = n - 1; i >= 0; i--) {
                if (nums[i] > nums[high]){
                    low = i;
                    break;
                }
            }

            int temp = nums[high];
            nums[high] = nums[low];
            nums[low] = temp;
            // 交换完之后，high 后面的需要在进行一次排序，从低到高，保证增加完高位之后，之后的位组成的数字是最小的，才能保证恰好大于
            // std::sort(nums.begin() + i, nums.end());
            // 其实不用专门排序，high 后面是降序的，只需要倒序一次就好了
            int start = i, end = n - 1;
            while (start < end) {
                int temp = nums[start];
                nums[start] = nums[end];
                nums[end] = temp;
                start++;
                end--;
            }
        }
    }
};
```

## 总结
这个题其实有点离散数学的东西，在开始做题之前一定要做好数学分析，其实代码很简单，主要是要做好数学分析
