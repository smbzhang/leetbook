## 题目描述（简单难度）
https://leetcode.com/problems/remove-duplicates-from-sorted-array/
![](/assets/026-1.jpg)

给定一个拍序好的数组，返回这个数组中有多少个不重复的元素。并且再原来的数组中进行修改，将所有不同的数全部存放到前面中，因为是个数组的引用，所以leetcode很容易检验前面几个数是不是整个数组中不同的数据。

## 思路
### 思路一
一开始我想的办法是交换，但是想了一段时间发现交换的情况很复杂。于是放弃了。。。。
### 思路二
这个思路其实类似快慢指针。

我们可以遍历一遍这个数组，然后把有多少不同的数计算出来，这样返回值就有了。时间复杂度O(log(n))，假设一共有k个不同的数

接下来，我们初始化一个 pos=1 ，指向当前需要修改的重复的数的位置。

记录i来遍历整个数组， 判断 nums[i] == nums[i - 1]; 如果相等的话，继续遍历，知道出现不相同的情况

nums[pos++] = nums[i]

执行这个操作，这样的话 i 始终领先于 pos 并不会出现重叠的情况。 总的时间复杂度也是 O(log(n))

## 代码
### C++ 实现

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int total = 0, ret = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (i == 0) total = 1;
            else if (nums[i] != nums[i - 1]) total++;
            else;
        }
        ret = total;
        int pos = 1;
        for (int i = 1; i < nums.size(); i++) {
            if (nums[i] != nums[i - 1]) nums[pos++] = nums[i];
        }
        return ret;
    }
};
```

**时间复杂度：** O(n)
**空间复杂度：** O(1) 

其实上面的循环可以合并成一个，让代码看起来更轻爽。（这是leetcode上面的人写的，很秀，和我想法一样但是比我的优雅 n 輩）

```
int removeDuplicates_2(vector<int>& A) {
    int count = 0, n = A.size();
    for(int i = 1; i < n; i++){
        if(A[i] == A[i-1]) count++;
        else A[i-count] = A[i];
    }
    return n-count;
}
```

## 总结
对于数组元素的操作，并不是只有交换这一种思路，这个题并不要求我们改变完数组之后保持数组的元素和原来一样，所以我们这里可以借助快慢指针的思想来做这个题。

**不要思维定势**