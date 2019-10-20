## 题目描述（中等难度）
https://leetcode.com/problems/divide-two-integers/
![](/assets/029-1.jpg)

这个题的难点就在于我们只能使用 int 型

## 思路

### 思路一

不能使用乘除法，那么我们可以使用加法。假设 A 是被除数， B 是除数。通过加法运算，我们可以计算 B自加多少次等于 A，或者刚好大于A。

在整个的过程中我们需要时刻保证不能有正数溢出的情况发生。

在实现上面的算法的过程中，我首先想的是把A，B转换成正数来进行运算，但是对于 INT_MIN (-2147483648) 来说是不能处理的。abs(INT_MIN) 还是等于 INT_MIN。转换成正数就要溢出。

所以转换思维，全部转换成负数来进行运算。计算式这样还是有两一个可能溢出的地方需要单独考虑。

返回值 ret，如果定义ret是int型，对于  A=-2147483648,B=-1。 这个case来说就会溢出了。

**时间复杂度：** O(N)
**空间复杂度：** O(1)

### 思路二
上面累加都是 B的自加，这样如果A=-2147483648,B=-1， 就需要运算2147483648次，会超时。

其实我们可以每次加B，B<<1,B<<1<<1,...这样指数的加，类似于竖式的除法。这样就能把时间复杂度降到O(logN)

同样的，我们还是把 B和A全部变成负数来运算

```
把B和A的运算结果的符号，保存到 sign
把B,A变成负数
int ret = 0
while A <= B:
    int i
    // 这个循环中。 B << i 可能会溢出，得做特殊处理
    for (i = 0; A <= B << i; i++) {
        // 保证 B << i    >    (1100 0000 0000 0000 0000 0000 0000 0000)
        if (B << i) < (1 << 31 | 1 << 30) {
            i++;
            break;
        }
    }
    // ret = ret - 1 << (i - 1)   其中 ret 可能会溢出， 当 i == 32 的时候, 也就是 dividend == INT_MIN && divisor == 1 或者 -1 的时候
    if (i == 32) return sign ? INT_MAX : INT_MIN;
    ret -= (1 << (i - 1)) 

if (ret <= INT_MIN) return INT_MAX;
if (!sign) {
    return ret;
}
return 0 - ret;
```

**时间复杂度：** O(log(n))
**空间复杂度：** O(1)


## 代码

### C++ 实现

#### 解法一

```
// O(dividend) 使用了 long long int， 来判断边界，不符合题目要求
int divide(int dividend_, int divisor_) {
    long long int dividend = dividend_, divisor = divisor_;
    if (divisor == 0) return INT_MAX;
    if (dividend == 0) return 0;
    long long int ret = 0;
    bool flag = true;
    if ((dividend < 0 && divisor > 0) || (dividend > 0 && divisor < 0)) flag = false;
    dividend = (dividend < 0) ? -dividend : dividend;
    divisor = (divisor < 0) ? -divisor : divisor;
    long long int sum = 0;
    for (; sum < dividend; sum += divisor) {
        ret++;
    }
    if (sum == dividend) ret = flag ? ret : -ret;
    else {
        ret = flag ? ret - 1 : -(ret - 1);
    }
    if (ret > INT_MAX) return INT_MAX;
    return ret;
}
```

解法一使用了 long 类型，不符合题目规范
**时间复杂度：** O(N)
**空间复杂度：** O(1)

#### 解法二

```
int divide_2(int dividend, int divisor) {
    if (divisor == 0) return INT_MAX;
    bool sign = (dividend < 0) ^ (divisor < 0);
    int ret = 0, sum = 0;
    if (divisor > 0) divisor = 0 - divisor;
    if (dividend > 0) dividend = 0 - dividend;
    for (; sum > dividend - divisor; sum += divisor) {
        ret++;
    }
    if (sum == dividend - divisor)
    {
        if (ret == INT_MAX) return INT_MAX;
        else ret += 1;
    }
    if (sign) {
        if (ret <= INT_MIN) return INT_MAX;
        ret = 0 - (ret);
    }
    return ret;
}
```

解法二，把A，B全部转换成负数来处理
**时间复杂度：** O(N)
**空间复杂度：** O(1)

### 解法三

```
// 使用负数进行计算的好处是不用判断 a = INT_MIN 的情况，这种情况下，正数很难处理
int divide_3(int dividend, int divisor) {
    int ret = 0;
    bool sign = (dividend > 0) ^ (divisor < 0);
    /*
        * 如果转换成正数来做的话，这个边界处理起来很困难，-214748364-21474836488, 一转换就要超 32 bit
    if (dividend < 0) {
        if (dividend == INT_MIN) 特殊处理
    }
    */
    if (dividend > 0) dividend = 0 - dividend;
    if (divisor > 0) divisor = 0 - divisor;
    while(dividend <= divisor) {
        int i = 0;
        // 如果直接判断 dividend <= divisor << i, divisor << i 会溢出，-2147483648， 1 这个例子就会一直死循环
        // for (; dividend <= divisor << i; i++);
        for (; dividend <= static_cast<unsigned int>(divisor) << i; i++) {
            if ((static_cast<unsigned int>(divisor) << i) < (1 << 31 | 1 << 30)) {
                i++;
                break;
            }
        }

        // ret = ret - 1 << (i - 1)   其中 ret 可能会溢出， 当 i == 32 的时候, 也就是 dividend == INT_MIN && divisor == 1 或者 -1 的时候
        if (i == 32) return sign ? INT_MAX : INT_MIN;
        ret -= 1 << (i - 1);
        dividend -= static_cast<unsigned int>(divisor) << (i - 1);
    }
    if (ret <= INT_MIN) return INT_MAX;
    if (!sign) {
        return ret;
    }
    return 0 - ret;
}
```
解法三呢，采用指数递减的方式简化了时间复杂度。

**时间复杂度：** O(log(n))
**空间复杂度：** O(1)
