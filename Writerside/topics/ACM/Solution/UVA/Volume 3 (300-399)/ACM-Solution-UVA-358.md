# UVA 358 - Don't Have A Cow

## 原题地址

[UVA 358 - Don't Have A Cow](https://onlinejudge.org/external/3/358.pdf)

## 🏷️ 题目类型

二分查找、几何 2D、两圆相交

## 📜 题目

**题意**

老麦克唐纳有一个农场，在这个农场上他有一头牛 - 还有一个半径为 $100$码的有围栏的圆形牧场。他计划把牛拴在牧场圆周上的一根柱子上。他希望牛能够吃到牧场
里三分之一的草。绳子应该有多长？您必须解决这个问题的一般化情况。

**输入**

输入以一个单独的正整数开始，该正整数独自位于一行，表示后面的案例数量，每个案例如下所述。此行之后是一个空行，并且在两个连续的输入之间也有一个空行。
编写一个程序，该程序将输入两个数字，$R$（半径： 一个在 $1$ 到 $1000$ 之间（包括 $1$ 和 $1000$）的整数）和 $P$（部分： 一个在 $0.0$ 到 $0.5$
之间（包括 $0.0$ 和 $0.5$）的实数），并解决老麦克唐纳的问题。老麦克唐纳应该使用多长的绳子，以使牛能够在半径为 $R$ 的圆形牧场中吃到 $P$ 比例的草。
将您的答案精确到两位小数。

**输出**

对于每个测试用例，以下面示例输出中所示的格式输出一条语句。数字 $P$ 和答案应四舍五入到两位小数。两个连续案例的输出将由一个空行分隔。

注意：$π$ 的推荐值为 $2 * acos(0)$ ，但任何与 $π$ 的精确值相差在 $1e-10$ 内的值都会得出预期答案。

**样例子输出中的值 $13.24$ 是故意不正确的。它仅包含在内是为了向您展示正确的格式!**

**样例输入**

```text
1
100 0.33
```

**样例输出**

```text
R = 100, P = 0.33, Rope = 90.64
```

## 🔍 分析

在该题中绳子越长，可以覆盖到的区域也就越大，所以可以对绳子的长度进行二分查找，然后通过求两个圆相交的面积即可。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/17.
// https://onlinejudge.org/external/3/358.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;
const double PI = acos(-1);

bool check(double R, double P, double r) {
    double theta1 = 2 * acos((R * R * 2 - r * r) / (2 * R * R));
    double theta2 = 2 * acos(r / (2 * R));
    double area1 = theta1 * R * R / 2;
    double area2 = theta2 * r * r / 2;
    double area3 = R * R * sin(theta1 / 2);
    double area = area1 + area2 - area3;

    return area / (PI * R * R) > P;
}

void solve(int testCase) {
    double R, P;
    cin >> R >> P;

    double left = 0, right = 2 * R;
    for (int i = 0; i < 100; i++) {
        double mid = (left + right) / 2;
        if (check(R, P, mid)) {
            right = mid;
        } else {
            left = mid;
        }
    }

    cout << fixed << setprecision(0) << "R = " << R << setprecision(2) << ", P = " << P << ", Rope = " << right << endl;
    if (testCase > 0) cout << endl;
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    cin >> T;
    while (T-- > 0) {
        solve(T);
    }

    return 0;
}
```