# UVA 465 - Overflow

## 🚀 原题地址

[UVA 465 - Overflow](https://onlinejudge.org/external/4/465.pdf)

## 🏷️ 题目类型

高精度

## 📜 题目

**题意**

编写一个程序，该程序读取由两个非负整数和一个运算符组成的表达式。确定这两个整数或表达式的结果是否太大而无法表示为“常规”有符号整数
（如果使用 `Pascal` 则为类型 `integer` ，如果使用 `C` 则为类型 `int` ）。

**输入**

未指定数量的行。每一行将包含一个整数、两个运算符 `+` 或 `*` 中的一个以及另一个整数。

**输出**

对于输入的每一行，打印输入内容，然后打印 $0$ - $3$ 行包含以下三个消息中尽可能多的适当消息：
`first number too big`， `second number too big`， `result too big`。

**样例输入**

```text
300 + 3
9999999999999999999999 + 11
```

**样例输出**

```text
300 + 3
9999999999999999999999 + 11
first number too big
result too big
```

## 🔍 分析

需要判断第一个数、第二个数以及这两个数的结果是否超过 32 位 int。可以使用高精度来做，但这道题情况比较简单，可以特殊判断。先对数字去除前导 $0$，然后
判断这个数的长度是否超过 $10$ 如果超过 $10$ 则说明这个数太大了。如果长度小于或等于 $10$，则只判断
这个数后 $10$ 位是否大于 $2147483647$，若大于则说明这个数太大了。当一个数太大了之后若另一个数不是 $0$ 则它们的结果也太大了。当两个数都不是大数
的情况可以使用 64 位整型来存储它们的值然后进行计算并判断结果的大小。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/27.
// https://onlinejudge.org/external/4/465.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

bool isZero(string s) {
    return count(s.begin(), s.end(), '0') == s.length();
}

bool check(string s) {
    reverse(s.begin(), s.end());
    while (s.size() > 1 && s.back() == '0') s.pop_back();
    reverse(s.begin(), s.end());
    long long num = stoll(s.substr(max(0, (int) (s.length() - 10))));
    return num > INT_MAX || s.length() > 10;
}

void solve() {
    string first, second, op;
    while (cin >> first >> op >> second) {
        cout << first << " " << op << " " << second << endl;

        if (check(first)) {
            cout << "first number too big" << endl;
        }

        if (check(second)) {
            cout << "second number too big" << endl;
        }

        if (check(first) || check(second)) {
            if (!isZero(first) && !isZero(second)) {
                cout << "result too big" << endl;
            }
            continue;
        }

        if (op == "+" && stoll(first) + stoll(second) > INT_MAX) {
            cout << "result too big" << endl;
        }

        if (op == "*" && stoll(first) * stoll(second) > INT_MAX) {
            cout << "result too big" << endl;
        }
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