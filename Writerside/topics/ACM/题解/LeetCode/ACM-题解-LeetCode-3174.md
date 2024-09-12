# LeetCode 3174. 清除数字

## 原题地址

[](https://leetcode.cn/problems/clear-digits)


<br/>

## 题目类型

栈

<br/>

## 代码

```C++
class Solution {
public:
    string clearDigits(string s) {
        string ans;
        for (char c : s) {
            if (isalpha(c)) {
                ans.push_back(c);
            } else {
                if (!ans.empty()) {
                    ans.pop_back();
                }
            }
        }

        return ans;
    }
};
```
