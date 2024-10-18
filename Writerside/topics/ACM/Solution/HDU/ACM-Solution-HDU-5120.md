# HDU 5120 - Intersection

## 原题地址

[HDU 5120 - Intersection](https://acm.hdu.edu.cn/showproblem.php?pid=5120)

## 🏷️ 题目类型

几何 2D、两圆相交

## 📜 题目

**题意**

马特是标志设计的超级粉丝。最近他爱上了由圆环组成的标志。以下图形是一些您可能知道的著名例子。

![](ACM-Solution-HUD-5120-01.jpg)

圆环是由两个共用中心的圆所界定的二维图形。这些圆的半径分别用 $r$ 和 $R$ 表示（$r \lt R$）。更多细节，请参考下面插图中的灰色部分。

![](ACM-Solution-HDU-5120-02.jpg)

马特刚刚在二维平面上设计了一个由两个相同大小的圆环组成的新标志。出于兴趣，马特想知道这两个圆环的交集面积。

**输入**

第一行仅包含一个整数 $T$（$T \leq 10^{5}$），表示测试用例的数量。对于每个测试用例，第一行包含两个整数 $r、R$（$0 \leq r \lt R \leq 10$）。

以下两行中的每一行都包含两个整数 $x_{i}、y_{i}$（$0 \leq x_{i}、y_{i} \leq 20$），表示每个圆环中心的坐标。

**输出**

对于每个测试用例，输出一行 `Case #x: y`，其中 $x$ 是用例编号（从 $1$ 开始），$y$ 是四舍五入到 $6$ 位小数的交集面积。

**样例输入**

```text
2
2 3
0 0
0 0
2 3
0 0
5 0
```

**样例输出**

```text
Case #1: 15.707963
Case #2: 2.250778
```

## 🔍 分析

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/17.
// https://acm.hdu.edu.cn/showproblem.php?pid=5120
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;
const double PI = acos(-1);


struct Circle {
    double x, y;
    double r;
};

double getDis(double x1, double y1, double x2, double y2) {
    return sqrt((x2 - x1) * (x2 - x1) + (y2 - y1) * (y2 - y1));
}

double getArea(Circle a, Circle b) {
    if (a.r < b.r) swap(a, b);

    double dis = getDis(a.x, a.y, b.x, b.y);

    if (dis >= a.r + b.r) return 0;

    if (dis <= a.r - b.r) return PI * b.r * b.r;

    double theta1 = 2 * acos((a.r * a.r + dis * dis - b.r * b.r) / (2 * a.r * dis));
    double theta2 = 2 * acos((b.r * b.r + dis * dis - a.r * a.r) / (2 * b.r * dis));
    double area1 = theta1 * a.r * a.r / 2;
    double area2 = theta2 * b.r * b.r / 2;
    double area3 = a.r * dis * sin(theta1 / 2);
    double area = area1 + area2 - area3;

    return area;
}

void solve(int testCase) {
    double r, R, x1, y1, x2, y2;
    cin >> r >> R >> x1 >> y1 >> x2 >> y2;

    Circle A = {x1, y1, R}, a = {x1, y1, r};
    Circle B = {x2, y2, R}, b = {x2, y2, r};

    double ans = getArea(A, B) - getArea(A, b) - getArea(a, B) + getArea(a, b);

    cout << "Case #" << testCase << ": ";
    cout << fixed << setprecision(6) << ans << endl;
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    cin >> T;
    for (int i = 1; i <= T; i++) {
        solve(i);
    }

    return 0;
}
```