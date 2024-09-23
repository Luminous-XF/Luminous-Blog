# LeetCode 1014. 最佳观光组合

## 原题地址

[](https://leetcode.cn/problems/best-sightseeing-pair)

<br/>

## 题目类型

动态规划

<br/>

## 代码

```C++
class Solution {
public:
    int maxScoreSightseeingPair(vector<int>& values) {
        int dp = values.front(), ans = INT_MIN;
        for (int i = 1; i < values.size(); i++) {
            ans = max(ans, dp + values[i] - i);
            dp = max(dp, values[i] + i);
        }

        return ans;
    }
};
```