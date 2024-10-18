# LeetCode 2207. 字符串中最多数目的子序列

## 原题地址

[](https://leetcode.cn/problems/maximize-number-of-subsequences-in-a-string)

<br/>

## 题目类型

枚举贡献、前缀和

<br/>

## 代码

时间复杂度: $O(n)$
<br/>
空间复杂度: $O(1)$

```C++
class Solution {
public:
    long long maximumSubsequenceCount(string text, string pattern) {
        long long cnt1 = 0, cnt2 = 0, ans = 0;
        for (char c : text) {
            if (c == pattern[1]) {
                ans += cnt1;
                cnt2++;
            } 
            if (c == pattern[0]) {
                cnt1++;
            }
        }

        ans += max(cnt1, cnt2);

        return ans;
    }
};
```