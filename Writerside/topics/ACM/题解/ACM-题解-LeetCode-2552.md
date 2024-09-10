# LeetCode 2552. 统计上升四元组

## 原题地址


[](https://leetcode.cn/problems/count-increasing-quadruplets)

<br/>

## 题目类型

枚举、树状数组

<br/>

## 代码

时间复杂度: $O(n^2)$ 
<br/>
空间复杂度: $O(n)$

```Go
class Solution {
public:
    long long countQuadruplets(vector<int>& nums) {
        const int n = (int) nums.size();
        
        int preCnt[n + 1];
        fill(preCnt, preCnt + n + 1, 0);

        long long ans = 0;
        for (int j = 0; j < n; j++) {
            int sufCnt = 0;
            for (int k = n - 1; k > j; k--) {
                if (nums[k] < nums[j]) {
                    ans += (long long) preCnt[nums[k]] * sufCnt;
                } else {
                    sufCnt++;
                }
            }
            
            for (int v = nums[j] + 1; v < n; v++) {
                preCnt[v]++;
            }
        }
        
        
        return ans;
    }
};
```