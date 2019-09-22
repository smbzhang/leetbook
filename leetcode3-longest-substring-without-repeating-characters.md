## 题目描述（中等难度）![](/assets/3.png)

这个题让找出最长的子串，该子串里面没有重复的字母

## 思路

看到这个题，首先想到的是动态规划来解，求出以i开头的最长无重复子串，从这些子串中找出最长的。其中以i开头的最长无重复子串是和以i+1开始的最长无重复子串有相应关系的，可以列出递推公式。

![](/assets/3.1.png)

```
1. 循环求出以i开头的最长无重复子串长度
2. 根据递推公式写出计算以i开头的无重复子串的最大长度程序。
```

## 代码

### C++实现

```
//动态规划递归求解, Memory Limit Exceeded
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int max = 0;
        for (int i = 0 ; i < s.length(); i++) {
            int len = lengthOfTheFirstSub(s, i);
            max = (len > max) ? len : max;
        }
        return max;
    }

    int lengthOfTheFirstSub(string str, int index) {
        string s = str.substr(index);
        if (s.length() <= 1) return s.length();
        if (s[0] == s[1]) {
            return 1;
        } else {
            int len = lengthOfTheFirstSub(str, index + 1);
            int n = s.substr(1, len).find(s[0]);
            if (n != string::npos) {
                return n + 1;
            } else {
                return len + 1;
            }
        }
    }
};
```

使用最简单的递归实现动态规划算法，leetcode显示内存超出限制，递归深度太高了，对于非常长的字符串。

```
//加入记录数组，提升查找速度, Memory Limit Exceeded 这解决不了内存的问题，递归层数太深了
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int max = 0;
        std::vector<int> records(s.length(), -1);
        for (int i = 0 ; i < s.length(); i++) {
            int len = lengthOfTheFirstSub(s, i, records);
            max = (len > max) ? len : max;
        }
        return max;
    }

    int lengthOfTheFirstSub(string str, int index, std::vector<int> &records) {
        if (records[index] != -1) {return records[index];}
        string s = str.substr(index);
        if (s.length() <= 1) {
            records[index] = s.length();
            return s.length();
        }
        if (s[0] == s[1]) {
            records[index] = 1;
            return 1;
        } else {
            int len = lengthOfTheFirstSub(str, index + 1, records);
            int n = s.substr(1, len).find(s[0]);
            if (n != string::npos) {
                records[index] = n + 1;
                return n + 1;
            } else {
                records[index] = len + 1;
                return len + 1;
            }
        }
    }
};
```

上面的程序通过加入记录数组来解决每次递归都重新计算重复的以i开头的最长无重复子串，这样可以减少递归的深度，并且减少时间复杂度。

