# LeetCode 2576. 求出最多标记下标

## 原题地址

[](https://leetcode.cn/problems/find-the-maximum-number-of-marked-indices)

<br/>

## 题目类型

双指针、贪心

<br/>

## 代码

时间复杂度: $O(n \log{n})$
<br/>
空间复杂度: $O(1)$

```C++
class Solution {
public:
    int maxNumOfMarkedIndices(vector<int> &nums) {
        sort(nums.begin(), nums.end());

        int ans = 0, n = (int) nums.size(), m = n / 2;
        for (int i = 0, j = m; i < m && j < n; i++, j++) {
            while (j < n && nums[i] * 2 > nums[j]) j++;
            if (j >= n) break;
            ans += 2;
        }

        return ans;
    }
};
```