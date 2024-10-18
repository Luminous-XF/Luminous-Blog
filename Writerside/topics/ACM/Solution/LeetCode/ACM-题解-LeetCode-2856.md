# LeetCode 2856. 删除数对后的最小数组长度

## 原题地址

[](https://leetcode.cn/problems/minimum-array-length-after-pair-removals)

<br/>

## 题目类型

思维、贪心

<br/>

## 代码

```C++
class Solution {
public:
    int minLengthAfterRemovals(vector<int> &nums) {
        int n = (int) nums.size(), x = nums[n >> 1];
        int cnt = (int) (upper_bound(nums.begin(), nums.end(), x) - lower_bound(nums.begin(), nums.end(), x));
        return max(cnt * 2 - n, n % 2);
    }
};
```