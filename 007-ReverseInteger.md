## 题目描述（简单难度）
https://leetcode.com/problems/reverse-integer/
![](/assets/007-1.jpg)

反转一个整数，溢出的话就返回0。

## 思路
关于整数溢出的处理我们有两种方式，具体可以参考[008-StringToIntegerAtoi](008-StringToIntegerAtoi.md).

但是这个题有个限制，我们只能使用 int类型的变量，所以这里不能使用long long int 判断溢出了。只能使用方法二。

## 代码

```
class Solution {
public:
    // 很丑陋的写法，不但使用了long long int 还使用了数组。
    int reverse(int x) {
        long long int max = (long long int)((1 << 31)) - 1;
        long long int num = (x >= 0) ? x : -x;
        bool flag = (x > 0) ? true : false;
        long long int quot = num / 10;
        int mod = num % 10;
        vector<int> records;
        records.push_back(mod);
        while (quot != 0) {
            mod = quot % 10;
            quot = quot / 10;
            records.push_back(mod);
        }
        long long int result = 0;
        int j = records.size() - 1;
        for (int i = 0; i < records.size(); i++) {
            result += pow(10, j--) * records[i];
            if (flag && result > max) return 0;
            if (!flag && result > max + 1) return 0;
        }
        if (flag) return result;
        else return -result;
    }
    // 下面的写法和上面同样的效果，但是不用新建数组，还是使用了 long long int ，题目规定只能使用 int
    int reverse_2(int x) {
        long long int res = 0;
        int quot = x;
        while (quot) {
            int mod = quot % 10;
            quot = quot / 10;
            res = res * 10 + mod;
            if (res > INT_MAX || res < INT_MIN) return 0;
        }
        return res;
    }

    // 下面不使用 long long int, 那么就需要改变判断是否溢出的方式
    int reverse_3(int x) {
        int res = 0;
        int quot = x;
        while (quot) {
            int mod = quot % 10;
            quot = quot / 10;
            int tmp = res * 10 + mod; // 这个 res * 10 + mod 可能会溢出
            if (tmp / 10 != res) return 0;  // 这种判断方式只有当int溢出的时候才会不相等,但是溢出的话leetcode会报错,又不允许使用long long
            res = tmp;
        }
        return res;
    }

    // 这个判断方法比较好
    int reverse_4(int x) {
        int res = 0;
        while (x) {
            int mod = x % 10;
            if (res > INT_MAX / 10 || (res == INT_MAX / 10 && mod > INT_MAX % 10)) return 0;
            if (res < INT_MIN / 10 || (res == INT_MIN / 10 && mod < INT_MIN % 10)) return 0;
            res = res * 10 + mod;
            x /= 10;
        }
        return res;
    }

};

```

## 总结
整数的反转，是不需要额外新建数组进行存储每一位的，关于溢出的判断可以参考[008-StringToIntegerAtoi](008-StringToIntegerAtoi.md)，还有上面的reverse_4

