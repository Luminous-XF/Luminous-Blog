# UVA 10170 - The Hotel with Infinite Rooms

## 原题地址

[UVA 10170 - The Hotel with Infinite Rooms](https://onlinejudge.org/external/101/10170.pdf)

## 🏷️ 题目类型

二分查找、数学

## 📜 题目

**题意**

哈尔瓦鲁蒂市有一家奇怪的酒店，房间数量无限。来到这家酒店的团体遵循以下规则：

a) 同一时间只有一个团体的成员可以租用酒店。

b) 每个团体在入住日的早上入住，并在退房日的晚上离开酒店。

c) 在前一个团体离开酒店的第二天早上，另一个团体会紧接着入住。

d) 新入住团体的一个非常重要的特点是，除非它是起始团体，否则它的成员比前一个团体多一个。您将获得起始团体的成员数量。

e) 一个有 n 个成员的团体在酒店停留 n 天。例如，如果一个由四名成员组成的团体在 8 月 1 日早上入住，它将在 8 月 4
日晚上离开酒店，而下一个由五名成员组成的团体将在 8 月 5 日早上入住并停留五天，依此类推。给定初始团体的规模，您需要找出在指定日期入住酒店的团体规模。

**输入**

输入在每一行都包含整数 $S ~(1 \leq S \leq 10000)$ 和 $D~(1 \leq D \lt 10^{15})$。$S$ 表示团队的初始规模，$D$
表示您需要找出第 $D$ 天（从 1 开始）在酒店的团队规模。所有输入和输出的整数都将小于 $10^{15}$。团队规模 $S$
意味着在第一天，一个由 $S$ 个成员组成的团队来到酒店并停留 $S$ 天，然后根据前面描述的规则会有一个由 $S + 1$
个成员组成的团队到来，依此类推。

**输出**

对于每一行输入，在单独一行上打印出第 D 天住在酒店的团体的规模。

**样例输入**

```text
1 6
3 10
3 14
```

**样例输出**

```text
3
5
6
```

## 🔍 分析

通过题目可以得知，团队人数是一个首项为 $S$，公差为 $1$ 的等差数列。假设第 $D$ 天团队人数为 $E$
，则有：$\sum_{i = S}^{E} i \geq D$，
因为 $\sum_{i = S}^{E}$ 是单调递增的，所以可以对 $E$ 进行二分查找。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/15.
// https://onlinejudge.org/external/101/10170.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

bool check(long long start, long long end, long long D) {
    return (start + end) * (end - start + 1) / 2 >= D;
}

void solve() {
    long long S, D;
    while (cin >> S >> D) {
        if (S >= D) {
            cout << S << endl;
            continue;
        }

        long long left = S, right = 44721360LL;
        while (left < right) {
            long long mid = left + right >> 1;
            if (check(S, mid, D)) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }

        cout << right << endl;
    }
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    //cin >> T;
    while (T-- > 0) {
        solve();
    }

    return 0;
}
```

