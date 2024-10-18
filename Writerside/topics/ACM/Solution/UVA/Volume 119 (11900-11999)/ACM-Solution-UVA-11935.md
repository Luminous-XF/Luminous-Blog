# UVA 11935 - Through the Desert

## 🚀 原题地址
[UVA 11935 - Through the Desert](https://onlinejudge.org/external/119/11935.pdf)

## 🏷️ 题目类型

模拟、二分查找

## 📜 题目

**题意**

假设您是一位试图穿越沙漠的探险家。在您的道路上有许多危险和障碍在等待着。您的生命取决于您那可靠的旧吉普车的油箱是否足够大。但它到底需要多大呢？
计算到达另一侧目的地所需的最小体积。以下事件描述了您的旅程：
- `Fuel consumption n`：意味着您的卡车每 $100$ 公里需要 $n$ 升汽油。$n$ 是在范围 $[1..30]$ 内的整数。 在您的旅程中，燃油消耗可能会发生变化，例如当您在山上或山下行驶时。

- `Leak`： 意味着您的卡车油箱被尖锐物体刺破。油箱将以每公里 $1$ 升燃油的速度开始泄漏汽油。多个泄漏叠加会导致卡车以更快的速度失去燃油。

然而，在这个沙漠中并非所有事件都是麻烦事。以下事件增加了您的生存机会：

- `Gas station`：让您加满油箱。

- `Mechanic`：修复您油箱中的所有泄漏。

- `Goal`：意味着您已安全到达旅程的终点！

**输入**

输入由一系列测试用例组成。每个测试用例最多包含 $50$ 个事件。每个事件由一个整数描述，即事件发生的距离（从起点测量，以公里为单位），后面跟着上述
类型的事件。在每个测试用例中，第一个事件的形式为 `0 Fuel consumption n`，最后一个事件的形式为 `d Goal`（$d$ 是到目标的距离）。事件按照
距离起点的非递减顺序排序给出，`Goal`事件始终是最后一个。在相同距离处可能有多个事件 - 按照给定的顺序处理它们。
输入以包含 `0 Fuel consumption 0`的行结束，不应处理此行。

**输出**

对于每个测试用例，打印一行包含能确保成功行程的最小可能油箱容量（以升为单位，小数点后保留三位精度）。

**样例输入**

```text
0 Fuel consumption 10
100 Goal
0 Fuel consumption 5
100 Fuel consumption 30
200 Goal
0 Fuel consumption 20
10 Leak
25 Leak
25 Fuel consumption 30
50 Gas station
70 Mechanic
100 Leak
120 Goal
0 Fuel consumption 0
```

**样例输出**

```text
10.000
35.000
81.000
```

## 🔍 分析

另油箱初始容量为 $0$，即所有油耗都先欠着，当加油的时候再恢复为 0。取行驶过程中欠的最大值即为最终结果。也可以对油箱的容量进行二分查找。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/19.
// https://onlinejudge.org/external/119/11935.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

bool solve() {
    string event;
    int before, flow;
    cin >> before >> event >> event >> flow;

    if (before == 0 && flow == 0) {
        return false;
    }

    double cap = 0, ans = INT_MAX;
    int cur, leak = 0;
    do {
        cin >> cur >> event;
        cap = cap - leak * (cur - before) - (double) flow / 100 * (cur - before);
        ans = min(ans, cap);
        before = cur;

        if (event == "Fuel") {
            cin >> event >> flow;
        } else if (event == "Leak") {
            leak++;
        } else if (event == "Gas") {
            cin >> event;
            cap = 0;
        } else if (event == "Mechanic") {
            leak = 0;
        } else if (event == "Goal") {
            break;
        }
    } while (true);

    cout << fixed << setprecision(3) << fabs(ans) << endl;

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