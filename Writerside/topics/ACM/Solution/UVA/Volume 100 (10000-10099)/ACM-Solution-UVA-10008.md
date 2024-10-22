# UVA 10008 - What’s Cryptanalysis?

## 🚀 原题地址
[UVA 10008 - What’s Cryptanalysis?](https://onlinejudge.org/external/100/10008.pdf)

## 🏷️ 题目类型

排序

## 📜 题目

**题意**

密码分析是破解他人密码书写的过程。这有时涉及对一段（加密）文本的某种统计分析。您的任务是编写一个对给定文本执行简单分析的程序。

**输入**

输入的第一行包含一个正十进制整数 $n$ 。这是输入中后续行的数量。接下来的 $n$ 行将包含零个或多个字符（可能包括空格）。这是必须分析的文本。

**输出**

每一行输出包含一个大写字母，后跟一个空格，然后是一个正十进制整数。该整数表示相应字母在输入文本中出现的次数。输入中的大写和小写字母视为相同。
不得计算其他字符。输出必须按降序计数顺序排序；也就是说，最频繁的字母在第一行输出，输出的最后一行表示出现频率最低的字母。如果两个字母出现的频率相同，
那么在字母表中靠前的字母必须在输出中靠前。如果一个字母未出现在文本中，则该字母不得出现在输出中。

**样例输入**

```text
3
This is a test.
Count me 1 2 3 4 5.
Wow!!!! Is this question easy?
```

**样例输出**

```text
S 7
T 6
I 5
E 4
O 3
A 2
H 2
N 2
U 2
W 2
C 1
M 1
Q 1
Y 1
```


## 🔍 分析

先统计出每个字母（不区分大小写）出现的次数，然后先按出现次数从大到小排序，出现次数相同的按字母字典序从小到大排序。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/22.
// https://onlinejudge.org/external/100/10008.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

struct Pair {
    char ch;
    int cnt;

    bool operator<(const Pair &rhs) const {
        if (cnt > rhs.cnt) return true;
        if (cnt < rhs.cnt) return false;
        return ch < rhs.ch;
    }
};

void solve() {
    int n;
    cin >> n;

    cin.ignore();

    unordered_map<char, int> book;
    while (n-- > 0) {
        string line;
        getline(cin, line);

        for (char c : line) {
            if (isalpha(c)) {
                c = toupper(c);
                book[c]++;
            }
        }
    }

    vector<Pair> v;
    for (auto &[ch, cnt] : book) {
        v.push_back({ch, cnt});
    }

    sort(v.begin(), v.end());

    for (auto &item : v) {
        cout << item.ch << " " << item.cnt << endl;
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