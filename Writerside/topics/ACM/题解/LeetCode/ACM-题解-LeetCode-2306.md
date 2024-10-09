# LeetCode 2306. 公司命名

## 原题地址

[](https://leetcode.cn/problems/naming-a-company)

<br/>

## 题目类型

枚举、位运算、Hash、字典树

<br/>

## 代码

时间复杂度: $O(nL \times 26)$
<br/>
空间复杂度: $O(nL)$

```C++
#pragma GCC optimize(3)

class Solution {
public:
    long long distinctNames(vector<string> &ideas) {
        for (string &idea: ideas) {
            insert(idea);
        }

        const int n = (int) ideas.size();
        int book[n];
        fill(book, book + n, 0);

        int cnt[26][26] = {0};
        for (int i = 0; i < n; i++) {
            string idea(ideas[i]);
            string s(idea);
            for (char c = 'a'; c <= 'z'; c++) {
                s[0] = c;
                if (!find(s)) {
                    cnt[idea[0] - 'a'][s[0] - 'a']++;
                    book[i] |= 1 << (c - 'a');
                }
            }
        }

        long long ans = 0;
        for (int i = 0; i < n; i++) {
            for (char c = 'a'; c <= 'z'; c++) {
                if ((book[i] >> (c - 'a') & 1) == 1) {
                    ans += cnt[c - 'a'][ideas[i][0] - 'a'];
                }
            }
        }

        return ans;
    }

    int idx = 0;
    int trie[500000][26];
    bool isExist[500000];

    void insert(string &s) {
        int p = 0;
        for (char c : s) {
            int x = c - 'a';
            if (trie[p][x] == 0) {
                trie[p][x] = ++idx;
            }
            p = trie[p][x];
        }
        isExist[p] = true;
    }

    bool find(string &s) {
        int p = 0;
        for (char c : s) {
            int x = c - 'a';
            if (trie[p][x] == 0) {
                return false;
            }
            p = trie[p][x];
        }
        return isExist[p];
    }
};
```

