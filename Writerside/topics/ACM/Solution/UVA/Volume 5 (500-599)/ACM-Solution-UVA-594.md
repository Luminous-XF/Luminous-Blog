# UVA 594 - One Little, Two Little, Three Little Endians

## 🚀 原题地址
[UVA 594 - One Little, Two Little, Three Little Endians](https://onlinejudge.org/external/5/594.pdf)

## 🏷️ 题目类型

位运算

## 📜 题目

**题意**

在不同的操作系统、操作系统版本和硬件平台上编写完全可移植的程序是一项具有挑战性的任务。遇到的困难之一是硬件制造商在如何在内存中存储整数数据方面所做
的决策导致的。由于这些表示在不同的机器上可能不同，如果不修改数据的存储方式或一个或多个平台处理数据的方式，通常无法共享二进制数据。幸运的是，硬件制
造商之间几乎普遍达成一致，可寻址内存应按 $8$ 位字节排序。对于需要超过 $8$ 位的整数数据值，例如大多数现代硬件上可用的典型 $2$ 字节、 $4$ 字节
和 $8$ 字节整 数类型，没有这样的协议，并且存在两种不兼容的存储方案。第一种将整数存储为连续的 $8$ 位字节组，其中最低有效字节占据组内的最低内存
位置，最高有效字节占据最高内存位置。第二种则正好相反；最低有效字节存储在组内的最高内存位置，最高有效字节存储在最低内存位置。计算行业分别将这些方
案称为小端模式和大端模式。还几乎普遍达成一致的是，有符号整数使用“二进制补码”表示，您可以假设情况就是如此。当在小端模式和大端模式的机器之间共享二进
制整数数据时，必须执行数据转换，其中涉及反转数据的字节。一旦字节被反转，硬件就会将整数正确地解释为来自另一端模式机器的原始值。此问题的目的是编写一
个程序，该程序将读取整数列表，并报告输入整数的二进制表示在另一端模式机器上所表示的整数。

**输入**

输入将由一系列整数组成。输入文件的末尾标志着列表的结束。所有输入的整数都可以表示为 $32$ 位有符号整数值。也就是说，输入的整数将在 $-2147483648$
到 $2147483647$ 的范围内。

**输出**

对于每个输入整数，应在输出文件中打印一行。这一行应包含输入整数，后跟短语 `'converts to'`，后跟一个空格，后跟另一端序值。

**样例输入**

```text
123456789
-123456789
1
16777216
20034556
```

**样例输出**

```text
123456789 converts to 365779719
-123456789 converts to -349002504
1 converts to 16777216
16777216 converts to 1
20034556 converts to -55365375
```

## 🔍 分析

假设输入的数为 $X$，$X$ 的 $4$ 个字节从高位到低位分别为 $Byte_{1}、~Byte_{2}、~Byte_{3}、~Byte_{4}$，那么结果就需要将字节由低到高排列，
即 $Byte_{4}、Byte_{3}、Byte_{2}、Byte_{1}$。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/24.
// https://onlinejudge.org/external/5/594.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

int swapBytes(int n) {
    int byte1 = (n >> 24) & 0xFF;
    int byte2 = (n >> 16) & 0xFF;
    int byte3 = (n >> 8) & 0xFF;
    int byte4 = n & 0xFF;

    return ((int) byte4 << 24) | ((int) byte3 << 16) | ((int) byte2 << 8) | ((int) byte1);
}

void solve() {
    int n;
    while (cin >> n) {
        int ans = swapBytes(n);
        cout << n << " converts to " << ans << endl;
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