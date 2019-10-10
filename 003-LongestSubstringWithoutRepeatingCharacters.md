## 题目描述（中等难度）
![](/assets/3.png)

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
//加入记录数组，提升查找速度, Memory Limit Exceeded 一定程度上解决内存问题
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

上面的程序通过加入记录数组来解决每次递归都重新计算重复的以i开头的最长无重复子串，这样可以减少递归的深度，并且减少时间复杂度。很遗憾对于长字符串还是内存超限了，所以我们必须干掉递归程序

```
// 改成递推求解，减少栈内存消耗
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.length();
        std::vector<int> records(n, 0);
        if (n <= 1) {return n;}
        records[n - 1] = 1;
        for (int i = n - 2; i >= 0; i--) {
            if (s[i] == s[i + 1]) {
                records[i] = 1;
            }else{
                int index = s.substr(i + 1, records[i + 1]).find(s[i]);
                if (index == string::npos) {
                    records[i] = records[i + 1] + 1;
                }else {
                    records[i] = index + 1;
                }
            }
        }
        int max = 0;
        for (int i = 0; i < records.size(); i++) {
            if (records[i] > max) {
                max = records[i];
            }
        }
        return max;
    }
};
```

改造成递推求解，这样时间复杂度就是O\(n\), 空间复杂度也是O\(n\)，顺利通过leetcode检查

### Go语言实现

```
func lengthOfLongestSubstring(s string) int {
    n := len(s)
    if (n <= 1) {return len(s)}
    var records []int = make([]int, n)
    records[n - 1] = 1
    for i := n -2; i >= 0; i-- {
        if (s[i] == s[i + 1]) {
            records[i] = 1;
        }else{
            if (!strings.Contains(s[i + 1: i + 1 + records[i +1]], string(s[i]))) {
                records[i] = records[i + 1] + 1;
            }else {
                index := strings.Index(s[i + 1: i + 1 + records[i +1]], string(s[i]))
                records[i] = index + 1
            }
        }
    }
    max := 0
    for _, num := range(records) {
        if num > max {max = num}
    }
    return max
}
```

### Python实现

```
class Solution(object):
    def lengthOfLongestSubstring(self, s):
        """
        :type s: str
        :rtype: int
        """
        n = len(s)
        if n <= 1:
            return len(s)
        records = [0] * n
        records[n - 1] = 1
        for i in range(n-2, -1, -1):
            if s[i] == s[i + 1]:
                records[i] = 1
            else:
                index = s[i + 1: i + 1 + records[i + 1]].find(s[i])
                if index == -1:
                    records[i] = records[i + 1] + 1
                else:
                    records[i] = index + 1
        max = 0
        for i in range(n):
            if records[i] > max:
                max = records[i]
        return max
```



