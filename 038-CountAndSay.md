## 题目描述（简单难度）
https://leetcode.com/problems/count-and-say/

![](/assets/038-1.jpg)
这个题目的意思是
```
1 
11 (1 个 1)
21 (2 个 1)
1211 (1 个 2， 1 个1)
```

## 思路

这个题主要是理解题意很麻烦。在知道了题目意思之后，可以采用递归的方式来解决。

f(n) = 遍历 f(n - 1) 字符串，并且输出结果

直接看代码

## 代码
### C++ 实现

```
class Solution {
public:
    string countAndSay(int n) {
        if (n <= 0) return "";
        if (n == 1) return "1";
        string str = countAndSay(n - 1);
        string result = "";
        int i = 0;
        while (i < str.size()) {
            char word = str[i];
            int num = 0;
            while(i < str.size() && str[i] == word) {
                num++;
                i++;
            }
            result += '0' + num;
            result += word;
        }
        return result;
    }
};
```


