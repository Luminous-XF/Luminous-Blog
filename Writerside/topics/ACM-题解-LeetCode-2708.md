# LeetCode 2708.一个小组的最大实力值

## 原题地址
[](https://leetcode.cn/problems/maximum-strength-of-a-group)

<br/>

## 题目类型
枚举、线性DP

<br/>

## 代码
```C++
class Solution {
public:
    long long maxStrength(vector<int> &nums) {
        const int n = nums.size();
        long long dp[2][n];
        for (int i = 0; i < 2; i++) {
            fill(dp[i], dp[i] + n, 0);
        }

        dp[0][0] = nums.front();
        dp[1][0] = nums.front();
        for (int i = 1; i < n; i++) {
            const long long num = nums[i];
            dp[0][i] = max(
                    max(dp[0][i - 1], num),
                    max(dp[0][i - 1] * num, dp[1][i - 1] * num)
            );
            
            dp[1][i] = min(
                    min(dp[1][i - 1], num),
                    min(dp[0][i - 1] * num, dp[1][i - 1] * num)
            );
        }

        return dp[0][n - 1];
    }
};
```