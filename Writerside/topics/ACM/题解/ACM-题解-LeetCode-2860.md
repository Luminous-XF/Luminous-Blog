# LeetCode 2860. 让所有学生保持开心的分组方法数

## 原题地址

[](https://leetcode.cn/problems/happy-students)


<br/>

## 题目类型

枚举


<br/>

## 代码
```C++
class Solution {
public:
    int countWays(vector<int> &nums) {
        sort(nums.begin(), nums.end());

        int ans = 0,  n = nums.size();
        for (int cnt = 0; cnt <= n; cnt++) {
            if ((cnt > 0 && nums[cnt - 1] >= cnt) ||
                (cnt < n && nums[cnt] <= cnt)) {
                continue;
            }
            ans++;
        }

        return ans;
    }
};
```
