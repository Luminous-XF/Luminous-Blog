# LeetCode 64. 最小路径和

## 原题地址

[](https://leetcode.cn/problems/minimum-path-sum)

<br/>

## 题目类型

动态规划

<br/>

## 代码

时间复杂度: $O(n ~ \times ~ m)$
<br/>
空间复杂度: $O(n ~ \times ~ m)$

```C++
class Solution {
public:
    int minPathSum(vector<vector<int>> &grid) {
        const int n = (int) grid.size(), m = (int) grid[0].size();

        int dp[n][m];
        dp[0][0] = grid[0][0];
        
        for (int i = 1; i < m; i++) {
            dp[0][i] = grid[0][i] +  dp[0][i - 1];
        }

        for (int i = 1; i < n; i++) {
            dp[i][0] = grid[i][0] + dp[i - 1][0];
        }

        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                dp[i][j] = min(dp[i - 1][j], dp[i][j - 1]) + grid[i][j];
            }
        }
        
        return dp[n - 1][m - 1];
    }
};
```