## 题目描述（简单难度）
![](/assets/014-1.jpg)
给定一组字符串，找出字符串的最长公共前缀，没有的话返回""

## 思路
找出所有字符串的公共前缀，肯定要遍历所有的字符串，所有字符串一个字符一个字符的比较，直到有字符匹配字符串相应位置失败。

在这之前，我们可以找出这组字符串中最短字符，按照最短字符的长度进行遍历。

## 代码
### C++ 实现
```
//https://leetcode.com/problems/longest-common-prefix/
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        int minlen = 10000;
        if (strs.size() == 0) return "";
        string result = "";
        for (auto tmp : strs) {
            minlen = (tmp.size() < minlen) ? tmp.size() : minlen;
        }
        for (int i = 0; i < minlen; i++) {
            char s = strs[0][i];
            for (auto str : strs) {
                if (str[i] != s)
                goto ret;
            }
            result += s;
        }
ret:
        return result;
    }
};
```
**时间复杂度：** O(n)
**空间复杂度：** O(1)

