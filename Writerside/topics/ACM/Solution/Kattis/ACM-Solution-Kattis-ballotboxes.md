# Kattis - Distributing Ballot Boxes

## 🚀 原题地址
[Kattis - Distributing Ballot Boxes](https://open.kattis.com/problems/ballotboxes)

## 🏷️ 题目类型

二分查找

## 📜 题目

**题意**

分发投票箱

今天，除了 SWERC'11，在西班牙还有另一个重要事件在举行，其重要性可与之相媲美：大选。该国每一位 $18$ 岁及以上的居民都被要求投票，以选出众议院和参
议院的代表。您不必担心所有法官会突然放弃他们的监督职责，因为投票并非强制。

政府有一些投票箱，是过去选举中使用过的。不幸的是，负责在城市之间分配箱子的人几个月前因财政限制被解雇了。因此，箱子分配给城市以及每个城市必须投票的人
员名单可能不是最佳的。您的任务是展示这项任务原本可以如何高效完成。

向城市分配投票箱的唯一规则是每个城市必须至少分配一个箱子。每个人必须在其先前分配到的箱子中投票。您的目标是获得一种分配方式，使分配到一个箱子中投票的
人数的最大值最小化。

在示例输入的第一个案例中，两个箱子分配给第一个城市，其余的分配给第二个城市，在最有效的分配中，正好有 $100,000$ 人被分配到每个（巨大的！）箱子中投
票。在第二个案例中，$1、2、2$ 和 $1$ 个投票箱分别分配给城市，并且来自第三个城市的 $1,700$ 人将被要求在他们村庄的两个箱子中的每一个中投票，使得
这些箱子在最佳分配中是最拥挤的。

**输入**

输入最多包含 $3$ 个测试用例。每个测试用例的第一行包含整数 $N$（$1 \leq N \leq 500000$），表示城市的数量，
以及 $B$（$N \leq B \leq 2000000$），表示投票箱的数量。接下来的 $N$ 行，每行包含一个整数 $a_{i}$（$1 \leq a_{i} \leq 5000000$），
表示第 $i$ 个城市的人口数量。
每个测试用例之间会有一个单独的空行。输入的最后一行将包含 $-1 -1$，不应进行处理。

**输出**

对于每个测试用例，你的程序应该输出一个整数，即最有效的分配方案中分配到一个投票箱的最大人数。

**样例输入**

```text
2 7
200000
500000

4 6
120
2680
3400
200

-1 -1
```

**样例输出**

```text
100000
1700
```

## 🔍 分析

最小化最大值，二分查找经典问题。对每个投票箱投票的最大人数进行二分查找。

## 💡 代码

```C++
//
// Created by Luminous on 2024/11/1.
// https://open.kattis.com/problems/ballotboxes
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 5e5 + 10;

int a[MAX_N];

bool check(int n, int m, int k) {
    for (int i = 0; i < n; i++) {
        m -= ceil((double) a[i] / k);
    }

    return m >= 0;
}

bool solve() {
    int n, m;
    cin >> n >> m;

    if (n == -1 && m == -1) {
        return false;
    }

    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    int left = 0, right = 5e6 + 10;
    while (left < right) {
        int mid = left + right >> 1;
        if (check(n, m, mid)) {
            right = mid;
        } else {
            left = mid + 1;
        }
    }

    int ans = right;
    cout << ans << endl;

    return true;
}

int main() {

    ios::sync_with_stdio(false);

    while (solve());

    return 0;
}
```