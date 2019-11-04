## 题目描述（中等难度）
https://leetcode.com/problems/multiply-strings/

![](/assets/043-1.jpg)

大数相乘没什么好说的

## 思路

乘法的话首先想到的是竖式相乘
![](/assets/043-2.jpg)

这里我想定义两个数组，来存放没喝数字相乘的余数(%10)和进位，最后在进行相加。

num1的长度是 n1， num2的长度是 n2

那么num1 * num2的结果的长度肯定不会大于 n1 + n2，所以定义array1和array2的长度是 n1 + n2

那么array中的数字会不会溢出呢？不会的因为字符串的最大长度是 110,所以 array中的数组最大是 9 * 110 = 990;

在计算array1 + array2 之后，遍历结果数组，首先去除前面的0，然后遍历到数组的最后。输出结果。


其实不需要建立两个数组，直接用array1，余数和进位直接加在相应的位置就好了。下面给出使用两个和一个数组的解法。

## 代码
### 思路一
### C++ 实现

使用两个数组
```
string multiply(string num1, string num2) {
    int n1 = num1.size(), n2 = num2.size();
    string result = "";
    if (n1 == 0 || n2 == 0) return result;
    if (num1 == "0" || num2 == "0") return "0";
    std::vector<int> array1(n1 + n2, 0), array2(n1 + n2, 0);
    int pos1 = n1 + n2, pos2 = n1 + n2 - 1;
    for (int i = n1 - 1; i >= 0; i--) {
        pos1--;
        pos2--;
        int m = 0;
        for (int j = n2 - 1; j >= 0; j--) {
            int a = num1[i] - '0', b = num2[j] - '0';
            array1[pos1 - m] += (a * b) % 10;
            array2[pos2 - m] += (a * b) / 10;
            m++;
        }
    }
    int high = 0;
    for (int i = n1 + n2 - 1; i >= 0; i--) {
        int sum = array1[i] + array2[i] + high;
        array1[i] = sum % 10;
        high = sum / 10;
    }
    int i = 0;
    while (array1[i] == 0) {
        i++;
    }
    for (; i < n1 + n2; i++) {
        result += array1[i] + '0';
    }

    return result;
}
```

使用一个数组
```
string multiply(string num1, string num2) {
    int n1 = num1.size(), n2 = num2.size();
    string result = "";
    if (n1 == 0 || n2 == 0) return result;
    if (num1 == "0" || num2 == "0") return "0";
    std::vector<int> array1(n1 + n2, 0);
    int pos1 = n1 + n2, pos2 = n1 + n2 - 1;
    for (int i = n1 - 1; i >= 0; i--) {
        pos1--; pos2--;
        int m = 0;
        for (int j = n2 - 1; j >= 0; j--) {
            int a = num1[i] - '0', b = num2[j] - '0';
            array1[pos1 - m] += (a * b) % 10;
            array1[pos2 - m] += (a * b) / 10;
            m++;
        }
    }
    int high = 0;
    for (int i = n1 + n2 - 1; i >= 0; i--) {
        int sum = array1[i] + high;
        array1[i] = sum % 10;
        high = sum / 10;
    }
    int i = 0;
    while (array1[i] == 0) {
        i++;
    }
    for (; i < n1 + n2; i++) {
        result += array1[i] + '0';
    }
    return result;
}
```


**时间复杂度：** O(n2)
**空间复杂度：** O(n) 