## 题目描述（中等难度）
![](/assets/016-1.jpg)
给定一个无序的整数数组，和target，找出三个数组中的数之和最接近target的结果。返回最接近的三个数的和。

## 思路
这个题和3Sum同根同源，先进性排序。正常遍历第一个数，然后采取首尾指针中间逼近的方法，确定另外两个数。

![](/assets/016-2.jpg)

## 代码
## C++ 实现
```
// 优化一下上面找第二个和第三个数的算法，上面是循环遍历找，这里使用两个指针进行查找
int threeSumClosest_2(vector<int>& nums, int target) {
    int result = nums[0] + nums[1] + nums[2];
    std::sort(nums.begin(), nums.end());
    int N = nums.size();
    int min_diff = INT_MAX;
    // 1. 先确定第一个数
    for (int i = 0; i < N - 2; i++) {
        if (i > 0 && nums[i] == nums[i - 1]) continue;
        // 2. 定义前后两个指针，往中间移动
        int m = i + 1, n = N -1;
        while (m < n) {
            int sum = nums[i] + nums[m] + nums[n];
            int diff = std::abs(sum - target);
            if (diff < min_diff) {
                min_diff = diff;
                result = sum;
            }
            if (sum < target) {
                m++;
            }else {
                n--;
            }
        }
    }
    return result;
}
```
**时间复杂度：** O(n2)
**空间复杂度：** O(1) 
