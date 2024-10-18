# POJ 2456 - Aggressive cows

## 🚀 原题地址

[POJ 2456 - Aggressive cows](http://poj.org/problem?id=2456)

## 🏷️ 题目类型

二分查找

## 📜 题目

**题意**

农夫约翰建造了一个新的长谷仓，有 $N$ 个（$2 \leq N \leq 100,000$
）畜栏。这些畜栏沿一条直线位于位置 $x_{1}, ~\cdots, ~x_{N} ~(0 \leq x_{i} \leq 1,000,000,000)$。

他的 $C$ 头（$2 \leq C \leq N$）奶牛不喜欢这个谷仓布局，一旦被放进畜栏就会互相攻击。为了防止奶牛互相伤害，$FJ$
想要把奶牛分配到畜栏中，使得任意两头奶牛之间的最小距离尽可能大。最大的最小距离是多少？

**输入**

- 第 $1$ 行：两个以空格分隔的整数：$N$ 和 $C$

- 第 $2$ 行至第 $N + 1$ 行：第 $i + 1$ 行包含一个整数摊位位置，$x_{i}$

**输出**

- 第 $1$ 行：一个整数：最大的最小距离

**样例输入**

```text
5 3
1
2
8
4
9
```

**样例输出**

```text
3
```

## 🔍 分析

对相邻两个牛舍直接的距离进行二分查找，找到最大的距离。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/18.
// http://poj.org/problem?id=2456
//

#pragma GCC optimize(3)

//#include <bits/stdc++.h>
#include <iostream>
#include <algorithm>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

long long a[MAX_N];

bool check(int n, int c, long long dis) {
    long long before = -dis;
    for (int i = 0; i < n; i++) {
        if (a[i] - before >= dis) {
            before = a[i];
            c--;
        }
    }

    return c <= 0;
}

void solve() {
    int n, c;
    cin >> n >> c;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    sort(a, a + n);

    long long left = 0, right = a[n - 1];
    while (left < right) {
        long long mid = left + right + 1 >> 1;
        if (check(n, c, mid)) {
            left = mid;
        } else {
            right = mid - 1;
        }
    }

    cout << left << endl;
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