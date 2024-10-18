# LeetCode 1436. 旅行终点站

## 原题地址

[](https://leetcode.cn/problems/destination-city)

<br/>

## 题目类型

Hash

<br/>

## 代码

时间复杂度: $O(n)$
<br/>
空间复杂度: $O(n)$

```C++
class Solution {
public:
    string destCity(vector<vector<string>>& paths) {
        unordered_map<string, bool> book;
        for (auto &path : paths) {
            book[path[0]] = true;
        }

        for (auto &path : paths) {
            if (!book[path[1]]) {
                return path[1];
            }
        }
        
        return "";
    }
};
```