## 题目描述（中等难度）
https://leetcode.com/problems/container-with-most-water/
![](/assets/011-1.jpg)

这个题是给定一组正整数(n >= 2)，每一个数在坐标上面表示高度，求这一组高度中，哪两个整数作为高度围住的水的容量最多。

## 思路
围住的水的容量的计算公式是 w * h（宽 * 高），一开始我尝试找到一个递推公式使用动态规划来解决，但是发现这个题并不复合最优子结构的性质，子状态最优并不能推出当前状态最优。

思路一：采用暴力搜索查的办法，计算所有的两两数字的组合组成的容量，然后找出最大的容量。时间复杂度 O(n2)

思路二：定义首尾两个指针,这样就保证了宽度最宽，然后计算首尾指针两个围城的容积。如果首比尾高那么，尾部前移，这样才能保证比前一个状态的容积可能大。
如果尾比首高，那么首部指针后移。如果相等的话就随意。这个过程中记录最大值返回。这种贪心实现。

## 代码
### C++ 实现
#### 解法一
暴力搜索
```
class Solution {
public:
    // 暴力搜索 O(n2)
    int maxArea(vector<int>& height) {
        int n = height.size();
        int max = 0;
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                int w = j - i;
                int h = std::min(height[i], height[j]);
                int con = w * h;
                max = con > max ? con : max;
            }
        }
        return max;
    }
};
```
**时间复杂度：** O(n2)
**空间复杂度：** O(1)

#### 解法二
首尾指针逼近的贪心算法
```
class Solution {
public:
    // https://leetcode.wang/leetCode-11-Container-With-Most-Water.html O(n)的时间复杂度
    int maxArea_2(vector<int>& height) {
        int n = height.size();
        int start = 0, end = n - 1;
        int max = 0;
        while(start < end) {
            int con = std::min(height[start], height[end]) * (end - start);
            max = (con > max) ? con : max;
            if (height[start] < height[end]) {
                start++;
            }else{
                end--;
            }
        }
        return max;
    }
};
```
**时间复杂度：** O(n)
**空间复杂度：** O(1)

## 总结
对于不符合最优子结构的题目，除了暴力法之外，还要寻找贪心的解法，找到巧妙的解决办法，这个题就是控制变量，控制住w，改变h。减少变量的纬度。