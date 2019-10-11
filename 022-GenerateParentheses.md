## 题目描述（中等难度）
https://leetcode.com/problems/generate-parentheses/
![](/assets/022-1.jpg)

## 思路
### 思路一
暴力求解法，每个位置上面有两个可能，穷举出每个位置的所有可能，然后判断最终的结果是不是合法。

时间复杂度比较高 O(2^2n), 一共有2n个坑位。

判断是不是合法：

- 是不是做括号等于右括号数
- 左边括号不能少于右边括号，在任意位置

### 思路二

改进上面的判断逻辑，上面是等全部的位置都填完之后在进行判断，其实可以在遍历的过程中进行判断剪枝打到缩短时间复杂度的情况。

在遍历的过程中，每次填入一个字符的时候判断

- 左括号数是不是 > 右括号数， 如果是的话 cur[pos] = ')', 否则 return，结束当前递归
- 左括号是不是 < n, 是的话 cur[pos] = '(', 否则return，结束当前递归


## 代码
### C++实现
#### 思路一
暴力法
```
class Solution {
public:
    // 暴力搜索，时间复杂度太高了，需要列举每个位置上的所有的两种情况

    vector<string> generateParenthesis(int n) {
        int pos = 0;
        vector<string> result;
        string cur(2*n, ' ');
        generateAll(n, pos, cur, result);
        return result;
    }

    void generateAll(int n, int pos, string cur, vector<string> &result) {
        // 判断当前结果是不是符合规则
        if (pos == n * 2) {
            int record = 0;
            for (int i = 0; i < cur.size(); i++) {
                if (cur[i] == '(') record++;
                else{
                    record--;
                    if (record < 0) break;
                }
            }
            if (record == 0) result.push_back(cur);
            return;
        }
        cur[pos] = '(';
        generateAll(n, pos+1, cur, result);
        cur[pos] = ')';
        generateAll(n, pos+1, cur, result);
    }
}
```

**时间复杂度：** O(2^2n)
**空间复杂度：** O(n) 

#### 思路二

class Solution {
public:
    // 优化判断条件不要在最后进行合法性的判断，进行剪枝
    vector<string> generateParenthesis_2(int n) {
        int pos = 0;
        vector<string> result;
        string cur(2*n, ' ');
        int right = 0, left = 0;
        generateAll(n, pos, cur, result, left, right);
        return result;
    }

    void generateAll(int n, int pos, string cur, vector<string> &result, int left, int right) {
        if (pos == 2 * n) {
            result.push_back(cur);
            return;
        }
        if (left < n) {
            cur[pos] = '(';
            generateAll(n, pos+1, cur, result, left + 1, right);
        }
        if (right < left) {
            cur[pos] = ')';
            generateAll(n, pos+1, cur, result, left, right + 1);
        }
    }
};
```
