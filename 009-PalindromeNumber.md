## 题目描述（简单难度）
https://leetcode.com/problems/palindrome-number/
![](/assets/009-1.jpg)

给定一个整数判断这个整数是不是一个回文整数。

## 思路

思路一：我们可以计算出这个整数的回文整数，然后和原数字进行比较。这里需要注意的一点是，如果反转的数字使用int进行存储，那么就需要进行溢出的判断，但是这里没有要求使用int存储，我们采用long long int存储，这样就不需要判断溢出了。

思路二：可以计算出这个数的个位十位百位。。。然后存入一个数组中最后判断这个数组是不是一个回文数组。可以使用首尾数组指针进行判断。

## 代码
## C++ 实现
### 解法一
计算出这个整数的回文整数，然后和原数字进行比较

```
class Solution {
public:
    // 解法二, 计算出反向的值直接进行比较, 这里不判断溢出
    bool isPalindrome_2(int x) {
        if (x < 0) return false;
        long long int num = 0, init = x;
        while(x){
            int mod = x % 10;
            num = num * 10 + mod;
            x /= 10;
        }
        return (num == init);
    }

};
```

**时间复杂度：** O(1)
**空间复杂度：** O(1)

## 解法二
```
class Solution {
public:
    // 解法一, 通过求余存储每个值，从右到左，然后对vector进行对称检验
    bool isPalindrome(int x) {
        if (x < 0) return false;
        vector<int> array;
        while(x) {
            int mod = x % 10;
            array.push_back(mod);
            x /= 10;
        }
        int i = 0, j = array.size() - 1;
        while(i < j) {
            if (array[i++] != array[j--]) return false;
        }
        return true;
    }
};
```

**时间复杂度：** O(n2)
**空间复杂度：** O(1)

## 总结
对于反转整数，需要注意溢出的情况，使用更大容量的整型long long int 来存放反转结果，可以免去判断溢出的逻辑。

不使用 反转整数的做法，还可以计算整数的每一个位数存储到数组，然后进行相应的逻辑判断。这样就会多使用空间。
