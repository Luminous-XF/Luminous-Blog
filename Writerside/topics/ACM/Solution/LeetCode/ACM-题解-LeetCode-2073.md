# LeetCode 2073. 买票需要的时间

## 原题地址

[](https://leetcode.cn/problems/time-needed-to-buy-tickets)

<br/>

## 题目类型

模拟

<br/>

## 代码

时间复杂度: $O(n)$
<br/>
空间复杂度: $O(1)$

```C++
class Solution {
public:
    int timeRequiredToBuy(vector<int>& tickets, int k) {
        int ans = 0;
        for (int i = 0; i < tickets.size() && tickets[k] != 0; i++, ans++) {
            tickets[i]--;
            if (tickets[i] > 0) {
                tickets.push_back(tickets[i]);
                if (i == k) {
                    k = tickets.size() - 1;
                }
            }
        }

        return ans;
    }
};
```