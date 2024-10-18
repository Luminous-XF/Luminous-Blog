# LeetCode 2516. 每种字符至少取 K 个

## 原题地址

[](https://leetcode.cn/problems/take-k-of-each-character-from-left-and-right)

<br/>

## 题目类型

双指针、滑动窗口

<br/>

## 代码

时间复杂度: $O(n \log{n})$
<br/>
空间复杂度: $O(n)$

```C++
#pragma GCC optimize(3)

class Solution {
public:
    int takeCharacters(string s, int k) {
        if (k == 0) return 0;
        if (k * 3 > s.length() ||
            count(s.begin(), s.end(), 'a') < k ||
            count(s.begin(), s.end(), 'b') < k ||
            count(s.begin(), s.end(), 'c') < k) {
            return -1;
        }

        const int n = (int) s.length();
        map<char, int*> sufCnt;
        for (char c = 'a'; c <= 'c'; c++) {
            sufCnt[c] = (int*) malloc((n + 1) * sizeof(int));
            sufCnt[c][n] = 0;
        }

        for (int i = n - 1; i >= 0; i--) {
            for (char c = 'a'; c <= 'c'; c++) {
                sufCnt[c][i] = sufCnt[c][i + 1];
            }
            sufCnt[s[i]][i]++;
        }

        int ans = INT_MAX;
        unordered_map<char, int> book;
        for (int i = -1; i < n; i++) {
            if (i >= 0) {
                book[s[i]]++;
            }
            
            int leftInx = INT_MAX;
            for (char c = 'a'; c <= 'c'; c++) {
                int v = max(0, k - book[c]);
                int inx = find(sufCnt[c], 0, n, v);
                leftInx = min(leftInx, inx);
            }

            if (leftInx != -1) {
                ans = min(ans, i + 1 + n - leftInx);
            }
        }

        return ans;
    }

    static int find(const int* arr, int l, int r, int v) {
        while (l < r) {
            int mid = l + r + 1 >> 1;
            if (arr[mid] >= v) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }

        return arr[l] == v ? l : -1;
    }
};
```