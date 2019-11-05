## 题目描述（困难难度）
https://leetcode.com/problems/trapping-rain-water/

![](/assets/042-1.jpg)

这个题给定一个数组，这个数组的大小表示在坐标轴中的高度。最后求出这一组坐标所能盛的水量。

## 思路

刚看到这个题的时候我的思路出现了偏差，我一直想找出一个递推公式，然后使用递归或者动态规划来实现。比如f(n)是以n为开头的存水量，或者是以n为结尾的存水量。或者f(m,n)是以m开头，n结尾的存水量。但是在经过多次思考之后发现并不能找出递归解，或者满足最优子结构的递推公式。

细挖题意的话，可以发现我们能够按照行或者列来进行计算每一行的储水量或者每一列的储水量，并且最后进行累加，便可以得出结果。
![](/assets/042-2.jpg)
![](/assets/042-3.jpg)

从上面两张图中我们很容易的就能发现求行和列的储水量的解法。

那么如何求解每一行的储水量呢？对于第n行，遍历列，如果当前列的高度小于n，并且其左边和右边的列都有高于当前列的，那么储水量+1；

怎么求每一列的储水量呢？

### 思路一
对于第n列的储水量，需要确定当前列的高度，以及左边列里面最高的 lmax和 右边列中最高的 rmax,如果 lamx 和 rmax 都大于当前列的高度，那么
当前列的储水量就是 std::min(lamx, rmax) - height[n]

**时间复杂度：** O(n2)
**空间复杂度：** O(1) 

### 思路二
对于思路一中的中间过程还可以进行优化，那就是求第n列左边和右边列的最大值。

lmax(i) = std::max(lmax(i - 1), height[i - 1])

rmax(i) = std::max(rmax(i + 1), height[i + 1])

上面的求解可以使用动态规划求解这两个数组。减少时间复杂度。

**时间复杂度：** O(nlogn)
**空间复杂度：** O(n)

### 思路三
采用双重指针来进行求解，代码机器精简。这是看leetcode题解发现的。具体直接看代码

## 代码
### 思路一
### C++实现
```
int trap(vector<int>& height) {
    int result = 0, n = height.size();
    for (int i = 1; i < n; i++) {
        int lmax = 0, rmax = 0;
        for (int m = 0; m < i; m++) {
            lmax = height[m] > lmax ? height[m] : lmax;
        }
        for (int m = i + 1; m < n; m++) {
            rmax = height[m] > rmax ? height[m] : rmax;
        }
        if (lmax > height[i] && rmax > height[i]) result += ((rmax < lmax) ? rmax : lmax) - height[i];
    }
    return result;
}
```

### 思路二
### C++实现
```
int trap_2(vector<int>& a){
    int n = a.size(), result = 0;
    vector<int> lmax(n, 0), rmax(n, 0);
    for (int i = 1; i < n; i++) {
        lmax[i] = a[i - 1] > lmax[i - 1] ? a[i - 1] : lmax[i - 1];
    }
    for (int i = n - 2; i >= 0; i--) {
        rmax[i] = a[i + 1] > rmax[i + 1] ? a[i + 1] : rmax[i + 1];
    }
    for (int i = 1; i < n; i++) {
        int l = lmax[i], r = rmax[i];
        if (a[i] < l && a[i] < r) {
            result += std::min(l, r) - a[i];
        }
    }
    return result;
}
```

### 思路三
### C++实现
```
int trap_3(vector<int>& height) {
    int l = 0, r = height.size()-1, level = 0, water = 0;
    while (l < r) {
        int lower = height[height[l] < height[r] ? l++ : r--];
        level = max(level, lower);
        water += level - lower;
    }
    return water;
}
```

## 总结
当发现最优子结构不存在，或者贪心解法不存在或者很难找到的时候，就需深入题意，拆解问题找出合适的解法。
在上面思路二的实现中我们使用动态规划，用空间换取了时间。