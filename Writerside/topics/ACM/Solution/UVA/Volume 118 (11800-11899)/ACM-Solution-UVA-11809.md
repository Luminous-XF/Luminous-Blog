# UVA 11809 - Floating-Point Numbers

## 🚀 原题地址
[UVA 11809 - Floating-Point Numbers](https://onlinejudge.org/external/118/11809.pdf)

## 🏷️ 题目类型

数学

## 📜 题目

**题意**

浮点数在计算机中的表示方式与整数不同。这就是为什么 $32$ 位浮点数能够表示约 $10^{38}$ 数量级的值，而 $32$ 位整数只能表示高达 $2^{32}$ 的值。

尽管计算机中浮点数的存储方式存在差异，但在这个问题中，我们假设浮点数以下面的方式存储：

![](ACM-Solution-UVA-11809-01.png)

浮点数有两个部分：尾数和指数。为尾数分配 $M$ 位，
为指数分配 $E$ 位。还有一位表示数字的符号（如果该位为 $0$，则数字为正，如果为 $1$，则数字为负），还有一位表示指数的符号（如果该位为 $0$，则指数
为正，否则为负）。尾数和指数的值共同构成浮点数的值。如果尾数的值为 $m$，则它满足 $\frac{1}{2} \leq m \lt 1$ 的约束。尾数的最左边一位必须始终
为 $1$ 以满足 $\frac{1}{2} \leq m \lt 1$ 的约束。所以这一位不存储，因为它总是 $1$。所以尾数中的位实际上表示二进制数小数点右侧的数字（不包括
小数点右侧的数字）。在上面的图中，我们可以看到一个浮点数，其中 $M = 8$ 且 $E = 6$。这个浮点数能够表示的最大值是（二进制）
$0.111111111_{2} \times 2^{111111_{2}}$ 。这个数字的十进制等效值是：$0.998046875 \times 2^{63} = 920535763834529382410$。
给定某个浮点数类型所能表示的最大可能值，您需要找出在该类型中为尾数（$M$）分配了多少位，为指数（$E$）分配了多少位。


**输入**

输入文件包含大约 $300$ 行输入。每行包含一个浮点数 $F$，它表示某个浮点类型可以表示的最大值。浮点数以十进制指数格式表示。因此，数字 $AeB$ 
实际上表示值 $A \times 10^{B}$。包含 `'0e0'` 的行终止输入。$A$ 的值将满足约束 $0 \lt A \lt 10$，并且小数点后正好有 $15$ 位数字。

**输出**

对于输入的每一行，生成一行输出。这一行包含 $M$ 和 $E$ 的值。您可以假设每个输入（除了最后一个）都有一个可能且唯一的解。您还可以假设输入将使
得 $M$ 和 $E$ 的值遵循约束条件：$9 \geq M \geq 0$ 且 $30 \geq E \geq 1$。此外，无需假设（$M + E + 2$）将是 $8$ 的倍数。

**样例输入**

```text
5.699141892149156e76
9.205357638345294e18
0e0
```

**样例输出**

```text
5 8
8 6
```

## 🔍 分析

由题意可知：$浮点数 = A \times 10^{B} = (1 - \frac{1}{2^{M + 1}}) \times 2^{2^{E} - 1}$

对式子两边取对数得：$\lg{A} + B = \lg(1 - \frac{1}{2^{M + 1}}) + (2^{E} - 1) \times lg{2}$

因为 $0 \lt A \lt 10$，所以 $lg{A} < 1$。又因为 $B$ 为整数，所以 $\lg{A}$ 为浮点数的小数部分，则有：

$B = \lfloor \lg(1 - \frac{1}{2^{M + 1}}) + (2^{E} - 1) \times lg{2} \rfloor$

$A = \lg(1 - \frac{1}{2^{M + 1}}) + (2^{E} - 1) \times lg{2} - B$

因为题目中 $M$ 和 $E$ 的范围比较小且一点存在唯一解，所以可以暴力枚举所有的 $M$ 和 $E$ 来验证结果。

**注意其中浮点数进行比较时 $EPS$ 的值不要取的 太小**。**

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/25.
// https://onlinejudge.org/external/118/11809.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

long long E[10][31];
double M[10][31];

void init() {
    for (int i = 0; i <= 9; i++) {
        for (int j = 1; j <= 30; j++) {
            double m = 1.0 - 1.0 / (1 << (i + 1));
            double e = (1 << j) - 1;
            double v = log10(m) + e * log10(2);
            E[i][j] = v;
            M[i][j] = pow(10, v - E[i][j]);
        }
    }
}

bool solve() {
    string line;
    cin >> line;

    if (line == "0e0") {
        return false;
    }

    double m = stod(line.substr(0, 17));
    long long e = stoll(line.substr(18));
    for (int i = 0; i < 10; i++) {
        for (int j = 1; j <= 30; j++) {
            if (E[i][j] == e && fabs(m - M[i][j]) < 1e-6) {
                cout << i << " " << j << endl;
                return true;
            }
        }
    }


    return true;
}

int main() {

    ios::sync_with_stdio(false);

    init();
    while (solve());

    return 0;
}
```