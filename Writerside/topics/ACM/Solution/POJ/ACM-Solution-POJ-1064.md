# POJ 1064 - Cable master

## 🚀 原题地址

[POJ 1064 - Cable master](http://poj.org/problem?id=1064)

## 🏷️ 题目类型

二分查找

## 📜 题目

**题意**

仙境的居民决定举办一场区域编程竞赛。评审委员会自愿参与，并承诺组织有史以来最公正的竞赛。决定使用“星型”拓扑结构为参赛者连接计算机——即将所有参赛者都连接到一个中央集线器。为了组织一场真正公正的竞赛，评审委员会主席下令让所有参赛者均匀地围绕集线器分布，且与集线器的距离相等。

为了购买网络电缆，评审委员会联系了当地的网络解决方案提供商，要求为他们出售指定数量的等长电缆。评审委员会希望电缆尽可能长，以使参赛者彼此之间的距离尽可能远。

该公司的电缆主管被分配了这项任务。他知道库存中每根电缆的长度精确到厘米，并且在被告知必须切割的长度时，他可以精确到厘米进行切割。然而，这次长度未知，电缆主管完全不知所措。

您需要通过编写一个程序来帮助电缆主管，确定从库存电缆中可以切割出的最大可能的电缆段长度，以获得指定数量的段。

**输入**

输入文件的第一行包含两个整数 $N$ 和 $K$，由一个空格分隔。$N$（$1 \leq N \leq 10000$
）是库存电缆的数量，$K$（$1 \leq K \leq 10000$）是所需件数。第一行之后是 $N$
行，每行一个数字，指定库存中每根电缆的长度（以米为单位）。所有电缆的长度至少为 $1$ 米，最多为 $100$
千米。输入文件中的所有长度均以厘米精度书写，小数点后恰好有两位数字。

**输出**

向输出文件写入电缆大师从库存电缆中可能切割出的件的最大长度（以米为单位），以获得所需数量的件。该数字必须以厘米精度书写，小数点后恰好有两位数字。

如果不可能切割出每个至少 $1$ 厘米长的所需数量的件，则输出文件必须包含单个数字 "$0.00$"（不含引号）。

**样例输入**

```text
4 11
8.02
7.43
4.57
5.39
```

**样例输出**

```text
2.00
```

## 🔍 分析

绳子越短可以切割到的根数就越多，所以可以对要切得绳子长度进行二分查找，找到在满足 $K$ 根绳子的情况下绳子的最大长度。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/18.
//

#pragma GCC optimize(3)

//#include <bits/stdc++.h>
#include <cmath>
#include <iomanip>
#include <iostream>
#include <algorithm>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

double a[MAX_N];

bool check(int n, int k, double len) {
    int cnt = 0;
    for (int i = 0; i < n; i++) {
        cnt += floor(a[i] / len);
    }

    return cnt >= k;
}

void solve() {
    int n, k;
    cin >> n >> k;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    double left = 0, right = *max_element(a, a + n);
    for (int i = 0; i < 100; i++) {
        double mid = (left + right) / 2;
        if (check(n, k, mid)) {
            left = mid;
        } else {
            right = mid;
        }
    }

    double ans = floor(left * 100) / 100;
    cout << fixed << setprecision(2) << ans << endl;
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

