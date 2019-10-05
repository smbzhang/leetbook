## 题目描述（简单难度）
![](/assets/020-1.png)
这个题让判断是不是合法的圆括号组合

## 思路
这个题可以使用栈来解决，遇到左边括号就入栈，遇到右边括号就检查栈顶元素，进行判断。

```
stack stk
1. 外层循环输入的字符串s, 循环变量 i：0 - n
    2. IF s[i] == '(' || '{' || '[' :
            入栈 s[i]
    ELSE:
            如果 stk 是空的， 那么直接返回 false
            否则的话 和栈顶元素进行比较，是不是 s[i] 对应的 左半边
                如果和栈顶匹配， 出栈继续
                否则返回 false
3. 如果 stk 为空，返会 true， 否则返回 false
```

## 代码
### C++ 实现

**解法一**
使用额外的map和set来存储用到的括号的映射关系，判断是不是左边类型的括号使用 set 来存储，判断栈顶元素和右边元素的映射关系使用 map 来实现
```
class Solution {
public:
    Solution() {
        mymap[')'] = '(';
        mymap[']'] = '[';
        mymap['}'] = '{';
        myset.insert('(');
        myset.insert('[');
        myset.insert('{');
    }
    // 解法一， 使用了额外的 map 和 set 空间
    bool isValid(string s) {
        std::stack<char> mystack;
        for (int i = 0; i < s.length(); i++) {
            if (myset.find(s[i]) != myset.end()) mystack.push(s[i]);
            else {
                if (mystack.empty()) return false;
                char top = mystack.top();
                if (mymap[s[i]] != top)
                    return false;
                else
                    mystack.pop();
            }
        }
        if (mystack.empty()) return true;
        else return false;
    }
private:
    std::map<char, char> mymap;
    std::set<char> myset;
};
```
**时间复杂度：** O(n)
**空间复杂度：** O(1)  map 和 set 的空间

**解法二**
优化一下上面的数据结构，其实不需要 set 结构来判断，因为输入一定是合法的六个字符，直接使用switch 判断或者if就好了。往stack中push的时候不要直接放入 s[i]，而是存放对应的右边括号，这样每次判断的时候就不要找映射关系了

```
class Solution {
    // 解法二： switch case 节省空间
    bool isValid_2(string s) {
        std::stack<char> mystack;
        for (int i = 0; i < s.length(); i++) {
            switch(s[i]) {
                case '(': mystack.push(']'); break;
                case '{': mystack.push('}'); break;
                case '[': mystack.push(']'); break;
                default:
                    if (mystack.empty() || mystack.top() != s[i]) return false;
                    else mystack.pop();
            }
        }
        return mystack.empty();
    }
};
```
**时间复杂度：** O(n)
**空间复杂度：** O(1)  没有额外空间的使用

## 总结
对于数量较少的固定集合，先不要忙着使用map或者set以及vector来存储，进行查找和存储，先想一下有没有其他的方式解决集合查找的问题，比如这个解法二中的 switch case。



