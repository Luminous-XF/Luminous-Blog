# UVA 10341 - Solve It

## 🚀 原题地址
[UVA 10341 - Solve It](https://onlinejudge.org/external/103/10341.pdf)

## 🏷️ 题目类型

二分查找、数学

## 📜 题目

**题意**

解方程式：$p \cdot e^{-x} + q \cdot \sin(x) + r \cdot \cos(x) + s \cdot \tan(x) + t \cdot x^{2} + u = 0$，其中 $0 \leq x \leq 1$ 。

**输入**

输入由多个测试用例组成，并以 `EOF` 结束。每个测试用例在一行中包含 $6$ 个整数：$p、q、r、s、t$ 和 $u$（其中 $0 \leq p、r \leq 20$ 且 $-20 \leq q、s、t \leq 0$）。输入文件中最多有 $2100$ 行。


**输出**

对于每组输入，应该有一行包含 $x$ 的值，精确到 $4$ 位小数，或者字符串“无解”，以适用的为准。

**样例输入**

```text
0 0 0 0 -2 1
1 0 0 0 -1 2
1 -1 1 -1 -1 1
```

**样例输出**

```text
0.7071
No solution
0.7554
```


## 🔍 分析

若函数在定义域中有解则函数区间端点值的结果相乘应该小于或等于 $0$，否则无解。此函数是一个单调递减函数，所有可以使用二分查找来求解。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/16.
// https://onlinejudge.org/external/103/10341.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;
const double EPS = 1e-14;

double fun(int p, int q, int r, int s, int t, int u, double x) {
    return p * exp(-x) + q * sin(x) + r * cos(x) + s * tan(x) + t * x * x + u;
}

void solve() {
    int p, q, r, s, t, u;
    while (cin >> p >> q >> r >> s >> t >> u) {
        if (fun(p, q, r, s, t, u, 0) * fun(p, q, r, s, t, u, 1) > 0) {
            cout << "No solution" << endl;
            continue;
        }

        double left = 0, right = 1;
        for (int i = 0; i < 100; i++) {
            double mid = (left + right) / 2;
            if (fun(p, q, r, s, t, u, mid) < 0) {
                right = mid;
            } else {
                left = mid;
            }
        }

        cout << fixed << setprecision(4) << setw(4) << setfill('0') << right << endl;
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