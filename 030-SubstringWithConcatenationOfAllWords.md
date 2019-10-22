## 题目描述（中等难度）
https://leetcode.com/problems/substring-with-concatenation-of-all-words/
![](/assets/030-1.jpg)

给定一组words和一个字符串s，然后找出这个words中所有字符的组合在s中出现的位置，返回这些位置。

(words 中的单词每一个长度都是相同的)

## 思路

### 思路一

刚看到这个题的时候，我忽略了words中单词的长度都相同这个条件，然后想通过计算出words中所有单词的不重复组合。

拿到这些不重复的组合之后，遍历这些组合，分别找出其中的字符串在s中的位置，然后返回。

计算words中的非重复组合的方法。采用全排列然后去重的算法。关于全排列的问题，我之后会写一个专门的章节。

全排列算法的时间复杂度应该和全排列的总数 O(n!) 一样。

在计算得到的words的非重复组合之后，便可以遍历s字符串，查找子串的位置。因为子串的所有位置都要找出，所以时间复杂度是， O(mnq), m是子串的长度，n是s的长度，q是words中子串的个数

这样的解法最终超时了。


### 思路二

仔细读题的话，可以发现，words中所有的子串的长度都是一样的。得到这个信息之后，可以在遍历s的时候直接和words中的每个子串进行比较，因为子串长度都是一样的，所以每次判断是不是匹配上某个子串就可以了。

怎么保证连续的匹配到所有的子串呢？ 我们可以新建一个hashmap<string, int>,存储words中的子串和出现次数。在遍历s的过程中，当前指针是i，那么根据words子串的长度 len, 依次匹配words中的单词，并且存放在一个hashmap中，并且计数。 在技术的过程中和之前的hamp对比，如果i到words,size()*len中出现了非子串的情况，或者子串的数量超过了之前记录的值，就返回并报错。

这样的时间复杂度就是 O(m*n) n:s的长度， m是子串的个数


## 代码

### C++ 实现

#### 解法一

```
vector<int> findSubstring(string s, vector<string>& words) {
    vector<int> result;
    words = permutation(words);
    for (int i = 0; i < words.size(); i++) {
        int pos = 0;
        pos = s.find(words[i], pos);
        while (pos != string::npos) {
            result.push_back(pos);
            pos = s.find(words[i], pos + 1);
        }
    }
    return result;
}


vector<string> permutation(vector<string> &strs) {
    vector<string> result;
    if (strs.size() == 0) {
        return result;
    }
    range(strs, 0, result);
    return result;
}

void range(vector<string> &strs, int start, vector<string> &result) {
    if (start == strs.size()) {
        string str;
        str.clear();
        for (int i = 0; i < strs.size(); i++) {
            str += strs[i];
        }
        // 使用 hashmap 去重
        if (this->records.find(str) == this->records.end()) {
            this->records.insert(str);
            result.push_back(str);
        }
        return;
    }
    for (int i = start; i < strs.size(); i++) {
        // 去重
        //bool flag = true;
        //for (int j = start; j < i; j++) {
        //    if (strs[j] == strs[start]) flag == false;
        //}
        //if (!flag) continue;
        swap(strs, start, i);
        range(strs, start + 1, result);
        swap(strs, start, i);
    }
}

void swap(vector<string> &strs, int i, int j) {
    string str = strs[i];
    strs[i] = strs[j];
    strs[j] = str;
}

std::unordered_set<string> records;

```


**时间复杂度：** O(m!) + O(nmlen)
**空间复杂度：** O(1)

#### 解法二

```
vector<int> findSubstring_2(string s, vector<string> &words) {
    vector<int> result;
    if (words.size() == 0 || s.length() == 0) return result;
    int len = words[0].size(), lens = len * words.size();
    int n = s.length();
    std::unordered_map<string, int> hmap;
    for (int i = 0; i < words.size(); i++) {
        if (hmap.find(words[i]) != hmap.end()) {
            hmap[words[i]]++;
        }else{
            hmap[words[i]] = 1;
        }
    }
    for (int i = 0; i <= n - lens; i++) {
        std::unordered_map<string, int> temp_map;
        bool flag = true;
        for (int j = 0; j < words.size(); j++) {
            string str = s.substr(i + len * j, len);
            if (hmap.find(str) == hmap.end()) {
                flag =  false;
                break;
            }else{
                if (temp_map.find(str) == temp_map.end()) temp_map[str] = 1;
                else temp_map[str]++;
                if (temp_map[str] > hmap[str]) {
                    flag = false;
                    break;
                }
            }
        }
        if (flag) {result.push_back(i);}
    }
    return result;
}
```


**时间复杂度：** O(m*n)
**空间复杂度：** O(n)

## 总结

看清题意，善用 hashmap，减少时间复杂度