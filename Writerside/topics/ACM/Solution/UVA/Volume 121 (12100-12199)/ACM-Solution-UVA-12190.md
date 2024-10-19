# UVA 12190 - Electric Bill

## 🚀 原题地址

[UVA 12190 - Electric Bill](https://onlinejudge.org/external/121/12190.pdf)

## 🏷️ 题目类型

二分查找

## 📜 题目

**题意**

现在是 $21004$ 年。电变得非常昂贵。最近，您的电力公司再次提高了电价。下面的表格显示了新的电价（用电量始终为正整数）：

范围 价格（疯狂瓦特小时）（阿梅里克斯）

| Range (Crazy-Watt-hour) | Price (Americus) |
|-------------------------|------------------|
| $1 \sim 100$            | $2$              |
| $101 \sim 10000$        | $3$              |
| $10001 \sim 1000000$    | $5$              |
| $\gt 1000000$           | $7$              |



这意味着，在计算要支付的金额时，前 $100$ 疯狂瓦特小时（$CWh$）的价格为每个 $2$ 阿梅里克斯；接下来的 $9900$ $CWh$（$101$ 到 $10000$ 之间）
的价格为每个 $3$ 阿梅里克斯，以此类推。例如，如果您消耗了 $10123$ $CWh$，您将需要支付 $2 \times 100 + 3 \times 9900 + 5 \times 123 = 30515$ 阿梅里克斯。

公司里邪恶的数学家们找到了一种赚更多钱的方法。他们不会告诉您消耗了多少能量以及需要支付多少钱，而是会向您展示两个与您和一个随机邻居相关的数字：

A：如果您和邻居的用电量一起计费，需要支付的总金额；

B：您和邻居账单金额之差的绝对值。

如果您无法弄清楚需要支付多少钱，您必须为这种“服务”再支付 $100$ 阿梅里克斯。您非常节俭，因此您确信自己的用电量不可能超过任何邻居。
所以，聪明的您知道可以计算出自己需要支付多少钱。

例如，假设公司告诉您以下两个数字：$A = 1100$ 和 $B = 300$。那么您和您邻居的用电量必须分别为 $150$ $CWh$ 和 $250$ $CWh$。总用电量为 $400$
$CWh$，那么 $A$ 就是 $2 \times 100 + 3 \times 300 = 1100$。您需要支付 $2 \times 100 + 3 \times 50 = 350$ 阿梅里克斯，而您的邻居必
须支付 $2 \times 100 + 3 \times 150 = 650$ 阿梅里克斯，所以 $B$ 是$|350 - 650| = 300$。

不愿意支付额外费用，您决定编写一个计算机程序来找出需要支付的金额。

**输入**

输入包含多个测试用例。每个测试用例由一行组成，包含两个整数 $A$ 和 $B$，用一个空格分隔，表示展示给您的数字（$1 \leq A, B \leq 10_{9}$）。
您可以假设总是有一个独特的解决方案，也就是说，恰好存在一对消耗产生这些数字。最后一个测试用例后面是一行，包含两个用一个空格分隔的零。

**输出**

对于输入中的每个测试用例，您的程序必须打印一行，包含一个整数，表示您需要支付的金额。

**样例输入**

```text
1100 300
35515 27615
0 0
```

**样例输出**

```text
350
2900
```

## 🔍 分析

假设与邻居的总耗电量为 $total$，用 $x$ 电的费用为 $f(x)$。则有 $f(total) = A$。然后求出与邻居的总耗电量 $total$，
可以直接算也可以使用二分查找。然后假设自己的耗电量为 $x$ ，邻居的耗电量为 $total - x$。则有 $|f(x) - f(total - x)| = B$，
因为题目中说 $x < total - x$，所以 $|f(x) - f(total - x)| = B$ 可以变为 $f(total - x) - f(x) = B$，此时就可以对 $x$ 值进行二分查找。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/19.
// https://onlinejudge.org/external/121/12190.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

long long getCost(long long v) {
    long long res = 0;
    if (v > 0) {
        res += 2 * min(v, 100LL);
        v -= 100LL;
    }
    if (v > 0) {
        res += 3 * min(v, 9900LL);
        v -= 9900LL;
    }
    if (v > 0) {
        res += 5 * min(v, 990000LL);
        v -= 990000LL;
    }
    if (v > 0) {
        res += 7 * v;
    }

    return res;
}

bool solve() {
    long long A, B;
    cin >> A >> B;

    if (A == 0 && B == 0) {
        return false;
    }

    long long totalLeft = 0, totalRight = 1e9;
    while (totalLeft < totalRight) {
        long long mid = totalLeft + totalRight >> 1;
        if (getCost(mid) >= A) {
            totalRight = mid;
        } else {
            totalLeft = mid + 1;
        }
    }

    long long total = totalRight;

    long long left = 0, right = total;
    while (left < right) {
        long long mid = left + right >> 1;
        if (getCost(total - mid) - getCost(mid) <= B) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }

    long long ans = getCost(right);
    cout << ans << endl;

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