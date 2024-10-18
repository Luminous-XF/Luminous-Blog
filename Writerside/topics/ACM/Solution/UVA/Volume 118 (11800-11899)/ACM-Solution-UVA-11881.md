# UVA 11881 - Internal Rate of Return

## 🚀 原题地址

[UVA 11881 - Internal Rate of Return](https://onlinejudge.org/external/118/11881.pdf)

## 🏷️ 题目类型

数学、二分查找

## 📜 题目

**题意**

在金融领域，内部收益率（`IRR`）是当净现值（`NPV`）等于零时投资的贴现率。形式上，给定 $T、~CF_{0}、~CF_{1}、~\cdots、~CF_{T}$ ，那么 `IRR` 
是以下方程的解：$NPV = CF_{0} ~ + ~\frac{CF_{1}}{(1 + IRR)} ~ + ~ \frac{CF_{2}}{(1 + IRR)^{2}} ~ + ~ \cdots ~ + ~ \frac{CF_{T}}{(1 + IRR)^{T}} = 0$。
您的任务是找到所有有效的 `IRR`。在这个问题中，初始现金流 $CF_{0} \lt 0$，而其他现金流均为正（对于所有 $i = 1, ~2, ~\cdots, ~CF_{i} \gt 0$）。重要：**`IRR` 可以为负**，但必须满足 $IRR \gt -1$ 。

**输入**

最多会有 $25$ 个测试用例。每个测试用例包含两行。第一行包含一个整数 $T ~(1 \leq T \leq 10)$，表示正现金流的数量。
第二行包含 $T + 1$ 个整数：$CF_{0}、~CF_{1}、~CF_{2}、~\cdots、~CF_{T}$，
其中 $CF_{0} \lt 0$，$0 \lt CF_{i} \lt 10000(i = 1, ~2, ~\cdots, ~T)$。当 $T = 0$ 时输入结束。

**输出**

对于每个测试用例，打印一行，包含 `IRR` 的值，四舍五入到两位小数。
如果不存在 `IRR` ，打印 `No`（不含引号）；如果存在多个 `IRR` ，打印 `Too many`（不含引号）。

**样例输入**

```text
1
-1 2
2
-8 6 9
0
```

**样例输出**

```text
1.00
0.50
```

## 🔍 分析

设 $f(IRR) ~ = ~ NPV$，该函数在 $IRR ~ \in ~ (-1, ~+\infty)$ 是单调递减的，且可以证明一定存在唯一解。所以 `No` 和 `Too many` 这两个
结果是不存在的！对 $IRR$ 进行二分查找就行。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/19.
// https://onlinejudge.org/external/118/11881.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 10 + 10;

int a[MAX_N];

bool check(int T, double IRR) {
    double res = 0, temp = IRR + 1  ;
    for (int i = 1; i <= T; i++) {
        res += a[i] / temp;
        temp = temp * (IRR + 1);
    }

    return res <= -a[0];
}

bool solve() {
    int T;
    cin >> T;

    if (T == 0) return false;

    for (int i = 0; i <= T; i++) {
        cin >> a[i];
    }

    double left = -1, right = INT_MAX;
    for (int i = 0; i < 100; i++) {
        double mid = (left + right) / 2;
        if (check(T, mid)) {
            right = mid;
        } else {
            left = mid;
        }
    }

    double ans = right;
    cout << fixed << setprecision(2) << ans << endl;

    return true;
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    //cin >> T;
    while (true) {
        if (!solve()) {
            break;
        }
    }

    return 0;
}
```