# LeetCode 2535. 数组元素和与数字和的绝对差

## 原题地址

[](https://leetcode.cn/problems/difference-between-element-sum-and-digit-sum-of-an-array)

<br/>

## 题目类型

模拟

<br/>

## 代码

时间复杂度: $O(n|num|)$
<br/>
空间复杂度: $O(1)$

```C++
class Solution {
public:
    int differenceOfSum(vector<int>& nums) {
        int sum1 = 0, sum2 = 0;
        for (int x : nums) {
            sum1 += x;
            sum2 += digitSum(x);
        } 

        return abs(sum1 - sum2);
    }

    int digitSum(int x) {
        int res = 0;
        while (x > 0) {
            res += x % 10;
            x /= 10;
        }
        return res;
    }
};
```

