## 题目描述（中等难度）
https://leetcode.com/problems/zigzag-conversion/
![](/assets/006-1.jpg)

这个题目的意思是，给定一个字符串s和一个整数numRows,遍历该字符串中的所有字符按照N型，N型的边长是numRows。

## 思路

在遍历s的时候定位出当前的字符应该存放到那个数组中，然后拼接数组。数组的纬度 numRows。

一开始我本想直接找到s的遍历指针i和对应数组下标的关系，但是尝试很长时间之后我发现我找不到。。。那么再仔细分析一下题目，画一下字符排列的路径。
![](/assets/006-2.jpg)
从上图中可以看出，除了第一次向下之外，其余的上下运动轨迹都遵循上 numDowns - 1 和 下 numDows -1 的规律，那这不正和数组下标对应上了吗.在这个过程中我们只需要定义一个变量path，
改变亮在遍历的过程中代表数组的存储下标，而且和i无关，只和遍历的次数有关。
![](/assets/006-3.jpg)
其实再仔细分析一下，第一次也不用搞特殊，我真是蠢了，这样也不用在边界上做特殊处理，path 也不会 <0 和 > numRows

```
path = 0
vector<string> records(3, "");
bool down = true
for i = 0; i < s.length; i++ 
    if (down)
        records[path++] += s[i];
        if (path == numDown - 1)
            down = false
    else
        records[path--] += s[i]
        if (path == 0)
            down = true;
 拼接records 里的字符串
```

## 代码
### C++ 实现

```
class Solution {
public:
    string convert(string s, int numRows) {
        if (numRows < 2) return s;
        vector<string> records(numRows, "");
        int n = s.length();
        bool down = true; // 方向
        int row = 0;
        for (int i = 0; i < n; i++) {
            if (down) {
                records[row] += s[i];
                row++;
                if (row == numRows - 1) {
                    down = false;
                }
            }else {
                records[row] += s[i];
                row--;
                if (row == 0) {
                    down = true;
                }
            }
        }
        string ret;
        for (int i = 0; i < numRows; i++) {
            ret += records[i];
        }
        return ret;
    }
};
```

## 总结
对于这种找规律的题和边界情况复杂的题，需要静下来分析，不要一有点思路就开始写代码了