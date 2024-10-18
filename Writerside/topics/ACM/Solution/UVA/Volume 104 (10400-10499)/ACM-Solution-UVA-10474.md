# UVA 10474 - Where is the Marble?

## 🚀 原题地址

[UVA 10474 - Where is the Marble?](https://onlinejudge.org/external/104/10474.pdf)

## 🏷️ 题目类型

二分查找

## 📜 题目

**题意**

拉朱和米娜喜欢玩弹珠。他们有很多上面写着数字的弹珠。一开始，拉朱会按照弹珠上所写数字的升序一个接一个地放置弹珠。然后米娜会让拉朱找出第一个带有某个数
字 的弹珠。她会数 $1 \cdots 2 \cdots 3$。拉朱答对得一分，如果拉朱答错米娜得分。经过一定次数的尝试游戏结束，得分最高的玩家获胜。今天轮到你扮演拉
朱了。作为聪明的孩子，你会借助电脑的帮助。但不要低估米娜，她写了一个程序来跟踪你给出所有答案所花费的时间。所以现在你必须编写一个程序，它将帮助你扮演
拉朱的角色。

**输入**

可能存在多个测试用例。测试用例的总数小于 $65$。每个测试用例都以 $2$ 个整数开始：$N$ 表示弹珠的数量，$Q$ 表示米娜要进行的查询数量。接下来的 $N$ 
行将包含写在 $N$ 个弹珠上的数字。这些弹珠的数字不会以任何特定的顺序出现。接下来的 $Q$ 行将有 $Q$ 个查询。请放心，输入的数字都不大于 $10000$
且都不是负数。输入以 $N = 0$ 和 $Q = 0$ 的测试用例结束。

**输出**

对于每个测试用例，输出该用例的序号。对于每个查询，打印一行输出。这一行的格式取决于查询编号是否写在任何弹珠上。以下是两种不同的格式：

- `x found at y`：如果编号为 $x$ 的第一个弹珠在位置 $y$ 被找到。位置编号为 $1、~2、~\cdots、~N$。

- `x not found`：如果编号为 $x$ 的弹珠不存在。查看示例输入的输出以获取详细信息。


**样例输入**

```text
4 1
2
3
5
1
5
5 2
1
3
3
3
1
2
3
0 0
```

**样例输出**

```text
CASE# 1:
5 found at 4
CASE# 2:
2 not found
3 found at 3
```

## 🔍 分析

先将 $N$ 个数按升序排序。对于每个查询 $x$，使用二分查找查询 $x$ 第一次出现的位置。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/19.
// https://onlinejudge.org/external/104/10474.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

int a[MAX_N];

int find(int x, int left, int right) {
    while (left < right) {
        int mid = left + right >> 1;
        if (a[mid] >= x) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }

    return a[right] == x ? right : -1;
}

bool solve(int testCase) {
    int n, q;
    cin >> n >> q;

    if (n == 0 && q == 0) return false;

    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    sort(a, a + n);

    cout << "CASE# " << testCase  << ":" << endl;
    while (q-- > 0) {
        int x;
        cin >> x;

        int index = find(x, 0, n - 1);
        if (index == -1) {
            cout << x << " not found" << endl;
        } else {
            cout << x << " found at " << (index + 1) << endl;
        }
    }

    return true;
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    //cin >> T;
    for (int i = 1;; i++) {
        if (!solve(i)) {
            break;
        }
    }

    return 0;
}
```