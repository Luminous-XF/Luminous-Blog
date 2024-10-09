# LeetCode 17. 电话号码的字母组合

## 原题地址

[](https://leetcode.cn/problems/letter-combinations-of-a-phone-number)

<br/>

## 题目类型

DFS

<br/>

## 代码

时间复杂度: $O(3^{m} ~ \times ~ 4^{n})$
<br/>
空间复杂度: $O(m ~ + ~ n)$

```C++
#pragma GCC optimize(3)

class Solution {
public:
    vector<string> letterCombinations(string digits) {
        vector<string> ans;
        if (digits.size() == 0) {
            return ans;
        }

        string s;
        dfs(0, s, digits, ans);

        return ans;
    }


    const char MAP[10][4] = {
            '-', '-', '-', '-',
            '-', '-', '-', '-',
            'a', 'b', 'c', '-',
            'd', 'e', 'f', '-',
            'g', 'h', 'i', '-',
            'j', 'k', 'l', '-',
            'm', 'n', 'o', '-',
            'p', 'q', 'r', 's',
            't', 'u', 'v', '-',
            'w', 'x', 'y', 'z'
    };

    void dfs(int step, string &s, string &digits, vector<string> &ans) {
        if (step == digits.length()) {
            ans.push_back(s);
            return;
        }

        const int v = digits[step] - '0';
        for (int i = 0; i < 4; i++) {
            if (MAP[v][i] == '-') continue;
            s.push_back(MAP[v][i]);
            dfs(step + 1, s, digits, ans);
            s.pop_back();
        }
    }
};
```