# LeetCode 36. 有效的数独

## 原题地址

[](https://leetcode.cn/problems/valid-sudoku)

<br/>

## 题目类型

Hash 标记、遍历


<br/>

## 代码

时间复杂度: $O(1)$
<br/>
空间复杂度: $O(1)$

```C++
class Solution {
public:
    bool isValidSudoku(vector<vector<char>>& board) {
        int rows[9] = {0};
        int cols[9] = {0};
        int subBoxs[3][3] = {0};
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] == '.') continue;
                int v = 1 << (board[i][j] - '0' - 1);
                if ((rows[i] & v) != 0 ||
                    (cols[j] & v) != 0 ||
                    (subBoxs[i / 3][j / 3] & v) != 0) {
                        return false;
                    }
                rows[i] |= v;
                cols[j] |= v;
                subBoxs[i / 3][j / 3] |= v;
            }
        }

        return true;
    }
};
```
