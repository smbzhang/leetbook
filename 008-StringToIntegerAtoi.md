## 题目描述（中等难度）
https://leetcode.com/problems/string-to-integer-atoi/
![](/assets/008-1.jpg)

这个题就是实现 atoi, 对于数字字符前面的只能出现空格和'-',  对于超出整数范围[−231, 231 − 1]的数，需要返回对应的 -231 或者 231

## 思路
- 去除字符串前缀的空格
- 判断第一个字符是不是 '-','+', 如果不是continue，是的话标记这是一个负(正)数
- 循环剩下的字符，直到出现非数字字符或者遍历完成。遍历的过程中计算 result，并且根据数字正负判断溢出。

## 代码
### C++ 实现
#### 解法一
返回的结果使用 int 存储，溢出的判断比较麻烦
```
class Solution {
public:
    int myAtoi(string str) {
        int n = str.size(), i = 0;
        int res = 0;
        bool flag = true;
        while(i < n && str[i] == ' ') i++;
        if (str[i] == '-' || str[i] == '+') {flag = (str[i] == '+') ? true : false; i++;}
        for(; i < n; i++) {
            if (str[i] > '9' || str[i] < '0') break;
            int num = str[i] - '0';
            if (flag && ((res > INT_MAX / 10) || (res == INT_MAX / 10 && num > INT_MAX % 10))) return INT_MAX;
            if (!flag && ((-res < INT_MIN / 10) || (-res == INT_MIN / 10 && -num <= INT_MIN % 10))) return INT_MIN;
            res = res * 10 + num; 
        }
        return (flag) ? res : -res;
    }
};

class Solution {
public:
    int myAtoi(string str) {
        int n = str.size(), i = 0;
        int res = 0;
        bool flag = true;
        while(i < n && str[i] == ' ') i++;
        if (str[i] == '-' || str[i] == '+') {flag = (str[i] == '+') ? true : false; i++;}
        for(; i < n; i++) {
            if (str[i] > '9' || str[i] < '0') break;
            int num = str[i] - '0';
            if (flag && ((res > INT_MAX / 10) || (res == INT_MAX / 10 && num > INT_MAX % 10))) return INT_MAX;
            if (!flag && ((-res < INT_MIN / 10) || (-res == INT_MIN / 10 && -num <= INT_MIN % 10))) return INT_MIN;
            res = res * 10 + num; 
        }
        return (flag) ? res : -res;
    }
};
```
**时间复杂度：** O(n)
**空间复杂度：** O(1)
#### 解法二
返回结果使用 long long int存储，溢出的判断简单
```
class Solution {
public:
    int myAtoi(string str) {
        int n = str.size(), i = 0;
        long long int res = 0;
        bool flag = true;
        while(i < n && str[i] == ' ') i++;
        if (str[i] == '-' || str[i] == '+') {flag = (str[i] == '+') ? true : false; i++;}
        for(; i < n; i++) {
            if (str[i] > '9' || str[i] < '0') break;
            int num = str[i] - '0';
            res = res * 10 + num;  // 如果 flag = false， 负值的最大值在这里会溢出，所以使用 long long int 来存储 res
            if (flag && res > INT_MAX) return INT_MAX;
            if (!flag && res - 1 > INT_MAX) return INT_MIN;
        }
        return (flag) ? res : -res;
    }
};
```
## 总结
这个题的难度不大，考验人的地方是如何使用比较优美的代码实现边界的判断等等。比如上面优雅的实现溢出的判断。

**整数溢出判断：**
如果result的类型是int， 判断quot是不是大于 INT_MAX/10, 然后判断个位数
```
if (flag && ((res > INT_MAX / 10) || (res == INT_MAX / 10 && num > INT_MAX % 10))) return INT_MAX;
if (!flag && ((-res < INT_MIN / 10) || (-res == INT_MIN / 10 && -num <= INT_MIN % 10))) return INT_MIN;
res = res * 10 + num;

 (-res == INT_MIN / 10 && -num <= INT_MIN % 10)  负数判断一定要注意，一定要 <=，因为res我们是按照正数处理最后再统一加符号，所以负数等于INT_MIN的时候就直接返回 INT_MIN，否则的话 res 就溢出了
```
直接让 res 带符号并且进行判断，就不需要单独处理上面的负数溢出了，参考 [007-ReverseInterger](007-ReverseInteger.md)


如果 result 使用 long long int 存储, 可以先计算 res然后在进行溢出判断
```
res = res * 10 + num;  // 如果 flag = false， 负值的最大值在这里会溢出，所以使用 long long int 来存储 res
if (flag && res > INT_MAX) return INT_MAX;
if (!flag && res - 1 > INT_MAX) return INT_MIN;
```
