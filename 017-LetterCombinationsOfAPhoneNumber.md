## 题目描述（中等难度）
![](/assets/017-1.jpg)
给定一个拨号键盘，输入一串数字，输出这串数字对应键盘上的字母的所有的排列组合。

## 思路
很明显这是一个全排列的问题。可以使用BFS或者DFS来进行遍历

使用深度优先遍历求解：
![](/assets/017-2.jpg)
使用BFS广度优先遍历实现,BFS的实现需要借助队列来实现
![](/assets/017-3.jpg)
使用笛卡尔积来进行实现
![](/assets/017-4.jpg)

## DFS解法代码
### C++ 实现
使用递归实现DFS,遍历到底部然后回溯
```
class Solution {
public:
    Solution() {
        records['2'] = "abc";
        records['3'] = "def";
        records['4'] = "ghi";
        records['5'] = "jkl";
        records['6'] = "mno";
        records['7'] = "pqrs";
        records['8'] = "tuv";
        records['9'] = "wxyz";
    }
    vector<string> letterCombinations(string digits) {
        vector<string> result;
        if (digits.length() == 0) {
            // 边界处理这个地方，必须返回一个 “” string， 不能直接返回一个空的 result, 否则结果为空
            // result.push_back("");
            return result;
        }
        if (digits.length() == 1) {
            for (int i = 0; i < records[digits[0]].length(); i++) {
                result.push_back(records[digits[0]].substr(i,1));
            }
            return result;
        }
        auto tmp = letterCombinations(digits.substr(1));
        for (int i = 0; i < records[digits[0]].length(); i++) {
            for (int j = 0; j < tmp.size(); j++) {
                result.push_back(records[digits[0]][i] + tmp[j]);
            }
        }
        return result;
    }

    // 补充一个 BFS 的解法
private:
    unordered_map<char, string> records;
};
```
## BFS解法代码
### C++ 实现
BFS解法，一层层遍历，最终循环输出结果，可以使用 队列来实现
```
// https://leetcode.com/problems/letter-combinations-of-a-phone-number/
// 很明显这是一个排列组合题， 列出所有的组合可能，使用深度优先遍历算法 DFS
class Solution {
public:
    Solution() {
        records['2'] = "abc";
        records['3'] = "def";
        records['4'] = "ghi";
        records['5'] = "jkl";
        records['6'] = "mno";
        records['7'] = "pqrs";
        records['8'] = "tuv";
        records['9'] = "wxyz";
    }
    // 补充一个 BFS 的解法,使用队列实现
    vector<string> letterCombinations(string digits) {
        std::queue<string> myqueue;
        std::vector<string> result;
        if (digits.length() < 1) return result;
        for (int i = 0; i < records[digits[0]].length(); i++)
        {
            string str = "";
            str += records[digits[0]][i];
            myqueue.push(str);
        }
        for (int i = 1; i < digits.length(); i++) {
            int len = myqueue.size();
            while(len--) {
                string str = myqueue.front();
                for (int j = 0; j < records[digits[i]].length(); j++) {
                    myqueue.push(str + records[digits[i]][j]);
                }
                myqueue.pop();
            }
        }
        while(!myqueue.empty()) {
            result.push_back(myqueue.front());
            myqueue.pop();
        }
        return result;
    }

private:
    unordered_map<char, string> records;
};
```

## 笛卡尔积实现
### Java 实现， 自定义相乘
https://leetcode.wang/leetCode-17-Letter-Combinations-of-a-Phone-Number.html
```
public List<String> letterCombinations(String digits) {
    List<String> ans = new ArrayList<String>();
    for (int i = 0; i < digits.length(); i++) {
        ans = mul(ans, getList(digits.charAt(i) - '0'));
    }
    return ans;

}

public List<String> getList(int digit) {
        String digitLetter[] = { "", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz" };
        List<String> ans = new ArrayList<String>();
        for (int i = 0; i < digitLetter[digit].length(); i++) {
            ans.add(digitLetter[digit].charAt(i) + "");
        }
          return ans;
    }
//定义成两个 List 相乘
public List<String> mul(List<String> l1, List<String> l2) {
    if (l1.size() != 0 && l2.size() == 0) {
        return l1;
    }
    if (l1.size() == 0 && l2.size() != 0) {
        return l2;
    }
    List<String> ans = new ArrayList<String>();
    for (int i = 0; i < l1.size(); i++)
        for (int j = 0; j < l2.size(); j++) {
            ans.add(l1.get(i) + l2.get(j));
        }
    return ans;
}
```

### python 的 笛卡尔积实现 - map reduce
见思路图三
