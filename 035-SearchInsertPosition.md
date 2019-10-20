## 题目描述（简单难度）
https://leetcode.com/problems/search-insert-position/
![](/assets/035-1.jpg)

## 思路

### 思路一

遍历这个数组，直到找到该数的位置，或者找到该数应该存放的位置。时间复杂度O(logn)


### 二分法求解

二分法找到这个数。但是这个数很可能不在这个数组里面。采用递归实现的二分法里面，我们可以单独处理边界条件，当传入的数组只有两个值的时候，我们就直接判断这个target应该存在的位置。

但是入股哦哦们总结一下的话发现并不用处理边界条件。如果我们正常按照二分法的流程走下来的话，如果target在数组中，肯定已经在遍历过程中找到了直接返回即可。如果二分法结束之后还没与找到，那么target应该存放的位置就应该是，最终的start的位置。


## 代码
### C++ 实现
#### 思路一
```
int searchInsert(vector<int>& nums, int target) {
        if (nums.size() == 0) return 0;
        return search(nums, target, 0, nums.size() - 1);
    }

    int search(const vector<int>& nums, const int &target, int start, int end) {
        if (start == end) {
            if (target > nums[start]) return start + 1;
            return start;
        }
        if (start + 1 == end) {
            if (target == nums[end]) return end;
            if (target > nums[end]) return end + 1;
            if (target <= nums[start]) return start;
            return end;
        }
        int mid = (start + end) / 2;
        if (target == nums[mid]) return mid;
        int ret;
        if (target < nums[mid]) ret = search(nums, target, start, mid - 1);
        if (target > nums[mid]) ret = search(nums, target, mid + 1, end);
        return ret;
    }
```

代码比较难看，需要处理的边界条件比较多


### 思路二

```
int searchInsert(vector<int>& nums, int target) {
    int start = 0, end = nums.size() - 1;
    while(start <= end) {
        int mid = (start + end) / 2;
        if (nums[mid] < target) {
            start = mid + 1;
        }
        else if (nums[mid] == target) return mid;
        else {
            end = mid - 1;
        }
    }
    return start;
}
```

经过总结之后写出的代码，非常的优雅。

## 总结
对于二分法的求解问题，需要多总结结果的共性不要一上来就着急处理边界问题。