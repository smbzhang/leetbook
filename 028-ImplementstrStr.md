## 题目描述（简单难度）
https://leetcode.com/problems/implement-strstr/
![](/assets/028-1.jpg)

给定一个字符串a, 和另一个字符串b。 返回字符串b在a中的下标。如果b不是a的子串，那么返回 -1， 如果b为空那么返回 0.

## 思路

```
a的长度是n， b的长度是m。 如果 n = 0，返回-1, 如果 m == 0, 返回 -1

指针i遍历a从0到n-m
{
    l = i; k = 0
    while (l < n && k < m && a[l++] == b[k]) k++;
    if (k == m)
        ret = i
        break
}
return ret

```

## 代码

### C++ 实现

```
class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.length(), m = needle.length();
        if (needle.empty()) return 0;
        if (haystack.empty()) return -1;
        int ret = -1;
        for (int i = 0; i <= n - m; i++) {
            int l = i, k = 0;
            while(l < n && k < m && haystack[l++] == needle[k]) k++;
            if (k == m) {
                ret = i;
                break;
            }
        }
        return ret;
    }
};
```

**时间复杂度：** O(nm)
**空间复杂度：** O(1) 