# UVA 1753 - Need for Speed

## 原题地址

[UVA 1753 - Need for Speed](https://onlinejudge.org/external/17/1753.pdf)

## 🏷️ 题目类型

二分查找

## 📜 题目

**题意**

希拉是一名学生，她开着一辆典型的学生车：老旧、缓慢、生锈且快要散架。最近，速度表上的指针掉了下来。她把它粘了回去，但可能粘的角度不对。因此，当速度表
读数为 $s$ 时，她的实际速度是 $s + c$，其中 $c$ 是一个未知常数（可能为负）。希拉仔细记录了最近的一次行程，并想用这个来计算 $c$。这次行程
由 $n$ 段组成。在第 $i$ 段，她行驶的距离为 $d_{i}$ ，并且整个段速度表读数为 $s_{i}$ 。整个行程花费的时间为 $t$。通过计算来帮助希拉求出 $c$。
请注意， 虽然希拉的速度表可能有负值读数，**但她在行程的每一段的实际速度都大于零**。

**输入**

输入文件包含几个测试用例，每个测试用例如下所述。输入的第一行包含两个整数 $n ~(1 \leq n \leq 1000)$
，即希拉旅程中的路段数量，以及 $t ~(1 \leq t \leq 10^{6})$，总时间。接下来是 $n$
行，每行描述希拉旅程的一个路段。这些行中的第 $i$ 行包含两个整数 $d_{i} ~(1 \leq d_{i} \leq 1000)$
和 $si ~(|s_{i}| \leq 1000)$，分别是旅程第 $i$ 个路段的距离和速度计读数。时间以小时为单位，距离以英里为单位，速度以 *
*$英里/小时$** 为单位。

**输出**

对于每个测试用例，在单独的一行中，显示以英里每小时为单位的常数 $c$ 。您的答案应该具有小于 $10−6$ 的绝对或相对误差。

**样例输入**

```text
3 5
4 -1
4 0
10 3
4 10
5 3
2 2
3 6
3 1
```

**样例输出**

```text
3.000000000
-0.508653377
```

## 🔍 分析

行驶路程固定，行驶速度越快，行驶的时间就越短。所以可以对 $c$ 的值进行二分查找。


## 💡 代码

```C++
//
// Created by Luminous on 2024/10/19.
// https://onlinejudge.org/external/17/1753.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 1e3 + 10;

double d[MAX_N];
double s[MAX_N];

bool check(int n, int t, double c) {
    double tt = 0;
    for (int i = 0; i < n; i++) {
        double ss = s[i] + c;
        if (ss <= 0) return false;
        tt += d[i] / (s[i] + c);
    }

    return tt <= t;
}

void solve() {
    int n, t;
    while (cin >> n >> t) {
        for (int i = 0; i < n; i++) {
            cin >> d[i] >> s[i];
        }

        double left = -2e6, right = 2e6;
        for (int i = 0; i < 100; i++) {
            double mid = (left + right) / 2;
            if (check(n, t, mid)) {
                right = mid;
            } else {
                left = mid;
            }
        }

        double ans = right;
        cout << fixed << setprecision(10) << ans << endl;
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