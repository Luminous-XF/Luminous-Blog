# HDU 2072 - 单词数

## 🚀 原题地址
[HDU 2072 - 单词数](https://acm.hdu.edu.cn/showproblem.php?pid=2072)

## 🏷️ 题目类型

数据结构、Hash

## 📜 题目

**题意**

`lily` 的好朋友 `xiaoou333` 最近很空，他想了一件没有什么意义的事情，就是统计一篇文章里不同单词的总数。
下面你的任务是帮助 `xiaoou333` 解决这个问题。

**输入**

有多组数据，每组一行，每组就是一篇小文章。每篇小文章都是由小写字母和空格组成，没有标点符号，遇到#时表示输入结束。

**输出**

每组只输出一个整数，其单独成行，该整数代表一篇文章里不同单词的总数。

**样例输入**

```text
you are my friend
#
```

**样例输出**

```text
4
```


## 🔍 分析

使用 `getline` 读取每行输入，然后使用 `stringstream` 读取每个单词并存入Hash表中，最终Hash表中元素的个数即为结果。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/19.
// https://acm.hdu.edu.cn/showproblem.php?pid=2072
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    string line;
    while (getline(cin, line)) {
        if (line == "#") {
            break;
        }

        stringstream ss(line);

        unordered_map<string, bool> book;
        string s;
        while (ss >> s) {
            book[s] = true;
        }

        cout << book.size() << endl;
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