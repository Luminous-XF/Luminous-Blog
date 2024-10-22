# HDU 1029 - Ignatius and the Princess IV

## 🚀 原题地址
[HDU 1029 - Ignatius and the Princess IV](https://acm.hdu.edu.cn/showproblem.php?pid=1029)

## 🏷️ 题目类型

快速排序、主元素查找

## 📜 题目

**题意**

“好，你还不算太糟，嗯……但你绝对通不过下一个测试。” feng5166 说。

“我会告诉你一个奇数 $N$，然后是 $N$ 个整数。它们之中会有一个特殊的整数，在我告诉你所有整数之后，你得告诉我哪个整数是特殊的。” feng5166 说。

“但这个特殊整数的特征是什么？” Ignatius 问道。

“这个整数会出现至少 $(N + 1) ~ / ~ 2$ 次。如果你找不到正确的整数，我就杀了公主，你也会成为我的晚餐，哈哈哈……” feng5166 说。

你能为 Ignatius 找到这个特殊整数吗？



**输入**

输入包含多个测试用例。每个测试用例包含两行。第一行由一个奇数整数 $N(1 \leq N \leq 999999)$组成，它表示 feng5166 要告诉我们主人公的整数数量。
第二行包含 $N$ 个整数。输入以文件结束为终止。

**输出**

对于每个测试用例，您只需输出一行，其中包含您找到的特殊数字。

**样例输入**

```text
5
1 3 2 3 3
11
1 1 1 1 1 5 5 5 5 5 5
7
1 1 1 1 1 1 1
```

**样例输出**

```text
3
5
1
```

## 🔍 分析

主元素查找。因为这个数出现了 $(N + 1) ~ / ~2$ 次，那么对这 $N$ 个数排序后中间那个数一定是答案。事实上可以不用排序，利用快速排序查找第 $K$ 大
数就可以在 $O(n)$ 时间复杂度内找到。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/22.
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 1e6 + 10;

int a[MAX_N];

int quickSort(int l, int r, int k) {
    if (l >= r) return a[l];

    int x = a[l + r >> 1], i = l - 1, j = r + 1;
    while (i < j) {
        do i++; while (a[i] < x);
        do j--; while (a[j] > x);
        if (i < j) swap(a[i], a[j]);
    }

    return k <= j ? quickSort(l, j, k) : quickSort(j + 1, r, k);
}

void solve() {
    int n;
    while (cin >> n) {
        for (int i = 0; i < n; i++) {
            cin >> a[i];
        }

        int ans = quickSort(0, n - 1, n / 2 + 1);
        cout << ans << endl;
    }
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
//    cin >> T;
    while (T-- > 0) {
        solve();
    }

    return 0;
}
```