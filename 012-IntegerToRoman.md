## 题目描述（中等难度）
https://leetcode.com/problems/integer-to-roman/
![](/assets/012-1.jpg)

这个题是第13题的一个逆向题，给定一个 1-3999 之间的整数，把它转换成一个 roman 字符串。

## 思路
这种转换问题需要建一个表进行查询，通过查表来进行最终结果的拼接。

我们对给定的数字进行除法运算，分别获得给定数字的千位、百位、十位、个位，这个过程中分别查表找到对应的roman字符串,加到返回的结果中。

那么这个表里面应该放什么内容呢？首先想到的是用map来简历映射关系，key存放3000，2000，1000，900，800 ... 3,2,1，value存放对应的roman串
```
std::map<int, string> records({
    {3000, "MMM"}, {2000, "MM"}, {1000, "M"},
    {900, "CM"},   {800, "DCCC"},{700, "DCC"},{600, "DC"},{500, "D"},{400, "CD"},{300, "CCC"},{200, "CC"},{100, "C"},
    {90, "XC"},    {80, "LXXX"}, {70, "LXX"}, {60, "LX"}, {50, "L"}, {40, "XL"}, {30, "XXX"}, {20, "XX"}, {10, "X"},
    {9, "IX"},     {8, "VIII"},  {7, "VII"},  {6, "VI"},  {5, "V"},  {4, "IV"},  {3, "III"},  {2, "II"},  {1, "I"}
});
```

这样每次进行查找的时候的时间复杂度是 O(logn)

当然还有更好的存储表的数据结构，数组！！！ 查询的时间复杂度是 O(1),如果建立一个一维的数组，因为要查询到3000，所以下标要到3000，这个存储容量是比较大的。

如果我们根据千位，百位，十位，个位进行纬度的划分，建立一个二维数组，就只需要 O(n) 的空间复杂度了

```
static const string s[4][10]=
{
    {"","I","II","III","IV","V","VI","VII","VIII","IX"},
    {"","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"},
    {"","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"},
    {"","M","MM","MMM"},
};
```

## 代码
### C++实现
#### 解法一
使用map建表
```
class Solution {
public:
    // 把所有的 能用到的拼接字符全部存入 map 中
    string intToRoman(int num) {
        std::map<int, string> records({
                {3000, "MMM"}, {2000, "MM"}, {1000, "M"},
                {900, "CM"},   {800, "DCCC"},{700, "DCC"},{600, "DC"},{500, "D"},{400, "CD"},{300, "CCC"},{200, "CC"},{100, "C"},
                {90, "XC"},    {80, "LXXX"}, {70, "LXX"}, {60, "LX"}, {50, "L"}, {40, "XL"}, {30, "XXX"}, {20, "XX"}, {10, "X"},
                {9, "IX"},     {8, "VIII"},  {7, "VII"},  {6, "VI"},  {5, "V"},  {4, "IV"},  {3, "III"},  {2, "II"},  {1, "I"}
        });
        int j = 1000;
        std:string result = "";
        while(j > 0) {
            int quot = num / j;
            num %= j;
            if (quot)
                result += records[quot * j];
            j /= 10;
        }
        return result;
    }
};
```
**时间复杂度：** O(n)
**空间复杂度：** O(1)

#### 解法二
使用二维数组建表查询
```
class Solution {
public:
    // 把所有的 能用到的拼接字符全部存入 map 中
    string intToRoman(int num) {
    // 不使用 map 使用数组来实现，借助数组的下标，这个人很秀，我还是太lj了~
    string intToRoman_2(int num) {
        static const string s[4][10]=
        {
            {"","I","II","III","IV","V","VI","VII","VIII","IX"},
            {"","X","XX","XXX","XL","L","LX","LXX","LXXX","XC"},
            {"","C","CC","CCC","CD","D","DC","DCC","DCCC","CM"},
            {"","M","MM","MMM"},
        };

        return s[3][num/1000%10] + s[2][num/100%10] + s[1][num/10%10] + s[0][num%10];
    }
};
```
**时间复杂度：** O(n)
**空间复杂度：** O(1)

## 总结
虽然上面的时间复杂度和空间复杂度相同，但是使用数组进行查询的速度要比红黑树实现的map更快，使用hashmap占用更多的空间，所以在以后建表的时候

**能使用数组和数组下标解决的优先使用该方式，不要以上来直接使用 map**