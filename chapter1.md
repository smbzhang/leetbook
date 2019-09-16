## 题目描述（简单难度）

```
Given an array of integers, return indices of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution, and you may not use the same element twice.

Example:

Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

```
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

## 解法一

> 简单粗暴，两重循环，外层遍历下标 i：\[0，n - 1\)，内层 j：\[i + 1，n - 1\]。n是数组的长度

### C++实现

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        vector<int> result;
        int n = nums.size();
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j <= n - 1; j++) {
                if (nums[i] + nums[j] == target) {
                    result.push_back(i);
                    result.push_back(j);
                    break;
                }
            }
        }
        return result;
    }
};
```

### Go实现

```
func twoSum(nums []int, target int) []int {
    var ret []int
    for i := 0; i < len(nums) - 1; i++ {
        for j := i + 1; j < len(nums); j++ {
            if nums[i] + nums[j] == target {
                ret = append(ret, i)
                ret = append(ret, j)
                return ret
            }
        }
    }
    return ret
}
```

### Python实现

```
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        ret = []
        n = len(nums)
        for i in range(0, n - 1):
            for j in range(i + 1, n):
                if nums[i] + nums[j] == target:
                    ret.append(i)
                    ret.append(j)
                    return ret
        return ret
```

时间复杂度：两层 for 循环，O（n²）

空间复杂度：O（1）

## 解法二

> 第二个循环是为了找出 target-nums\[i\] 的值，代替循环我们可以使用hash map来存储数组信息 key：数组的值 value：数组的下标

### C++实现

```

```

## 总结



