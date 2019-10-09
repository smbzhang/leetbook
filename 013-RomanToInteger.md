## 题目描述（简单难度）
![](/assets/013-1.jpg)
这个题目是给定一个roman字符串，计算出对应的数值

## 思路
入手这个题目的核心思想就是查表，根据给定的roman字符和数字的对应关系，从头到尾遍历roman字符串，然后根据查表对应的数字，加到result中，最终返回即可。

那么这个表里面应该放什么内容呢？首先给定的 1,5,10,50,100,500,1000 肯定要在表里面。还有几个特殊的两个字符的也要放在表里面, 4:IV,9:IX,40:XL,90:XC,400:CD,900:CM

有了这个表就可以进行遍历查询了，查询的过程中注意是不是匹配 4，9，40等这几个两个字符的特殊数字。

## 代码
### C++ 实现

```
class Solution {
public:
    int romanToInt(string s) {
        std::map<string, int> records({
            {"M", 1000}, {"D", 500}, {"C", 100}, {"L", 50}, {"X", 10}, {"V", 5}, {"I", 1},
            {"CM", 900}, {"CD", 400}, {"XC", 90}, {"XL", 40}, {"IX", 9}, {"IV", 4}
        });
        int i = 0, n = s.length(), result = 0;
        while (i < n) {
            int j = i + 1;
            std::string str;
            if (j < n) {
                str += s[i];
                str += s[j];
                if (records.find(str) != records.end()) {
                    result += records[str];
                    i += 2;
                    continue;
                }
            }
            str.clear();
            str += s[i++];
            result += records[str];
        }
        return result;
    }
};
```
**时间复杂度：** O(n)
**空间复杂度：** O(1)

## 总结
使用查表解决转换问题

