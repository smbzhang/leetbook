## 题目描述（中等难度）
https://leetcode.com/problems/longest-palindromic-substring/
![](/assets/005-1.jpg)

最长回文子串，这个题是非常经典的一个题。

## 思路
最长回文子串的解有三种方法。另外还有一种变种解法。
- 马拉车算法 （没搞懂）
- 中心扩展法
![](/assets/005-3.jpg)
中心扩展法的核心思想就是，遍历所有的字符，并且以该字符为中心向外扩展，并且判断是否为回文串直到非回文或者扩展达到s的边界。记录下以该点为中心的最长回文子串的状态，start，end。

需要注意的是，对于 baad，如果默认中心为一个数（也就是奇数）那么就会判断失误，所以，我们还需要判断中心是偶数的情况。

最后综合奇数中心和偶数中心遍历出来的结果进行比较，然后得出最终结果。

- 动态规划的方法 
![](/assets/005-2.jpg)
遍历所有的两点之间的字符串是不是回文字符串，并且记录是回文字符串的状态，start和end。最后返回。这里需要建立records二维数组。

初始化该数组的时候，不能只初始化单个字符是回文串的部分，两个字符的也需要初始化，否则的话，两个的字符串需要在循环中单独处理，比较麻烦，代码也不优雅

- 使用两个字符串的最长公共子序列求解

**转换思路**如果我们转换一下思路，将s字符串反转，这个题就成了找到两个字符串的最长公共子序列，这就成了另一个题了，这个题就相对简单，可以使用动态规划来解决了。

## 代码
### C++实现
#### 解法一
动态规划的办法
```
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.length();
        if (n == 0) {return "";}
        if (n == 1) {return s;}
        vector<vector<bool> > records(n, vector<bool>(n, false));
        int start = 0, max = 1;
        // 子串长度为1的回文长度是1，子串长度是2的，且两个字符相同的，回文长度是2
        for (int i = 0; i < n; i++) {
            records[i][i] = true;
            if (i < n  - 1 && s[i] == s[i + 1]) {
                records[i][i+1] = true;
                start = i;
                max = 2;
            }
        }
        // 子串长度从 3 - n
        // longestPalindrome(i, j) 以及 longestPalindrome(i + 1, j -1) 的关系
        for (int len = 3; len <= n; len++) {
            for (int i = 0; i + len <= n; i++) {
                if (s[i] == s[i + len - 1] && records[i + 1][i + len - 1 - 1]) {
                    records[i][i + len - 1] = true;
                    start = i;
                    max = len;
                }
            }
        }
        return s.substr(start, max);
    }
```
**时间复杂度：** O(n2)
**空间复杂度：** O(n) 

#### 解法二
中心扩展的办法

```
class Solution {
public:
    // 中心扩展法
    string longestPalindrome_2(string s) {
        int n = s.length();
        if (n < 2) {return s;}
        bool is_even = false;
        int center = 0;
        int max = 1;
        for (int i = 0; i < n - 1; i++) {
            if (s[i] == s[i + 1]) { // 中心是偶数的情况
                int start = i;
                int end = i + 1;
                while(start >= 0 && end < n && s[start] == s[end]) {
                    int len = end - start + 1;
                    if (len > max) {
                        max = len;
                        is_even = true;
                        center = i;
                    }
                    start--;
                    end++;
                }
            }
            { // 中心是基数的情况, 基数和偶数并不是互斥的情况，基数是一定要考虑的
               int start = i, end = i;
                while(start >= 0 && end < n && s[start] == s[end]) {
                    int len = end - start + 1;
                    if (len > max) {
                        max = len;
                        is_even = false;
                        center = i;
                    }
                    start--;
                    end++;
                }
            }
        }
        if (!is_even) {
            return s.substr(center - max/2, max);
        }else{
            return s.substr(center - max/2 + 1, max);
        }
    }
};
```
**时间复杂度：** O(n2)
**空间复杂度：** O(1) 

## 总结
这个题的解法很多，其中最亮眼的还是新建一个反转字符串，然后求这两个字符串的最长公共子序列，这就成了另一个比较简单的题。掌握一个题的解法就可以解决其他的变种题！！！