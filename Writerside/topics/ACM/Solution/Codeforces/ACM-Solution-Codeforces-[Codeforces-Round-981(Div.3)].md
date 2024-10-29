# Codeforces Round 981 (Div. 3)

## A. Sakurako and Kosuke

[原题地址](https://codeforces.com/contest/2033/problem/A)

### 🏷️ 题目类型

思维

### 📜 题目

**题意**

小樱子和小佑介决定在坐标线上用一个点玩一些游戏。该点当前位于位置 $x = 0$。他们将轮流进行，小樱子先开始。

在第 i 步，当前玩家将把点沿某个方向移动 $2 ~\cdot~ i ~ - ~1$ 个单位。小樱子总是将点沿负方向移动，而小佑介总是将其沿正方向移动。

换句话说，会发生以下情况：

1. 小樱子将点的位置改变为 $-1$，现在 $x = -1$

2. 小佑介将点的位置改变为 $3$，现在 $x = 2$

3. 小樱子将点的位置改变为 $-5$，现在 $x = -3$

4. ⋯

只要点的坐标的绝对值不超过 $n$，他们就会继续玩。更正式地说，当 $-n \leq x \leq n$ 时，游戏继续。可以证明游戏总会结束。

你的任务是确定谁将进行最后一步。

**输入**

第一行包含一个整数 $t (1 \leq t \leq 100)$ ——小樱子和小佑介玩的游戏数量。

每个游戏由一个数字 $n (1 \leq n \leq 100)$ 描述——定义游戏结束条件的数字。

**输出**

对于 $t$ 个游戏中的每一个，输出一行该游戏的结果。如果小樱子进行最后一步，输出 `"Sakurako"`（不带引号）；否则输出 `"Kosuke"`。

**样例输入**

```text
4
1
6
3
98
```

**样例输出**

```text
Kosuke
Sakurako
Kosuke
Sakurako
```


### 🔍 分析

$n$ 为奇数则最后一步是 `Kosuke`，否则是 `Sakurako`。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/29.
// https://codeforces.com/contest/2033/problem/A
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    int n;
    cin >> n;
    cout << ((n & 1) == 1 ? "Kosuke" : "Sakurako") << endl;
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    cin >> T;
    while (T-- > 0) {
        solve();
    }

    return 0;
}
```

<br/>

## B. Sakurako and Water

[原题地址](https://codeforces.com/contest/2033/problem/B)

### 🏷️ 题目类型

枚举

### 📜 题目

**题意**

在她与小介的旅程中，樱子和小介发现了一个可以表示为 n×n 大小矩阵的山谷，其中在第 $i$ 行和第 $j$ 列的交汇处是一座高度为 $a_{ij}$的山。
如果 $a_{ij} \lt 0$，那么那里是一个湖泊。

小介非常怕水，所以樱子需要帮助他：

- 凭借她的魔法，她可以选择一个方形的山区，并将该区域主对角线上的每座山的高度恰好增加 $1$ 。

更正式地说，她可以选择一个左上角位于 $(i,j)$ 且右下角位于 $(p,q)$ 的子矩阵，使得 $p - i = q - j$。然后，对于所有满足 $0 \leq k \leq p - i$ 的 $k$，
她可以将第 $(i + k)$ 行和第 $(j + k)$ 列交汇处的每个元素增加 $1$。

确定樱子必须使用她的魔法的最少次数，以使没有湖泊。

**输入**

第一行包含一个整数 $t (1 \leq t \leq 200)$ — 测试用例的数量。

每个测试用例的描述如下：

- 每个测试用例的第一行由一个数字 $n (1 \leq n \leq 500)$ 组成。

- 接下来的 $n$ 行中的每一行都由用空格分隔的 $n$ 个整数组成，它们对应于山谷中山的高度 $a[-10_{5} \leq a_{ij} \leq 10_{5}]$。

保证所有测试用例中 $n$ 的总和不超过 $1000$。

**输出**

对于每个测试用例，输出樱子必须使用她的魔法的最少次数，以使所有湖泊消失。


**样例输入**

```text
4
1
1
2
-1 2
3 0
3
1 2 3
-2 1 -1
0 0 -1
5
1 1 -1 -1 3
-3 1 4 4 -4
-1 -1 3 0 -5
4 5 3 -3 -1
3 1 -3 -1 5
```

**样例输出**

```text
0
1
4
19
```

### 🔍 分析

枚举每条对角线，每条对角线中最小的负数的绝对值的和即为结果，

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/29.
// https://codeforces.com/contest/2033/problem/B
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 5e2 + 10;

int a[MAX_N][MAX_N];

int fun(int x, int y, int n) {
    int res = 0;
    while (x < n && y < n) {
        if (a[x][y] < 0) {
            res = max(res, -a[x][y]);
        }
        x++, y++;
    }
    return res;
}

void solve() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < n; j++) {
            cin >> a[i][j];
        }
    }

    int ans = 0;
    for (int i = 0; i < n; i++) {
        ans += fun(0, i, n);
        ans += fun(i, 0, n);
    }

    ans -= fun(0, 0, n);

    cout << ans << endl;
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    cin >> T;
    while (T-- > 0) {
        solve();
    }

    return 0;
}
```

<br/>

## C. Sakurako's Field Trip

[原题地址](https://codeforces.com/contest/2033/problem/C)

### 🏷️ 题目类型

动态规划

### 📜 题目

**题意**

即使在大学里，学生也需要放松。这就是为什么小樱的老师决定去实地考察。众所周知，所有的学生都将排成一行。索引为 $i$ 的学生有一些感兴趣的话题，
描述为 $a_{i}。作为老师，您希望尽量减少学生队伍的干扰。

队伍的干扰定义为具有相同感兴趣话题的相邻人数。换句话说，干扰是满足 $a_{j} = a_{j} + 1$ 的索引 $j$ 的数量（$1 \leq j \lt n$）。

为了做到这一点，您可以选择索引 $i$ （$1 \leq i \leq n$）并交换位置 $i$ 和 $n - i + 1$ 的学生。您可以执行任意次数的交换。

您的任务是确定通过执行上述操作任意次数可以实现的最小干扰量。

**输入**

第一行包含一个整数 $t$ （$1 \leq t \leq 10^{4}$） - 测试用例的数量。

每个测试用例由两行描述。

- 第一行包含一个整数 $n$ （$2 \leq n \leq 10^{5}$） - 学生队伍的长度。

- 第二行包含 $n$ 个整数 $a_{i}$ （$1 \leq a_{i} \leq n$） - 队伍中学生感兴趣的话题。

保证所有测试用例中 $n$ 的总和不超过 $2 \cdot 10^{5}$ 。

**输出**

对于每个测试用例，输出您可以实现的队伍的最小可能干扰。

**样例输入**

```text
9
5
1 1 1 2 3
6
2 1 2 2 1 1
4
1 2 1 1
6
2 1 1 2 2 4
4
2 1 2 3
6
1 2 2 1 2 1
5
4 5 5 1 5
7
1 4 3 5 1 1 3
7
3 1 3 2 2 3 3
```

**样例输出**

```text
1
2
1
0
0
1
1
0
2
```

### 🔍 分析

**_TODO ..._**

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/29.
// https://codeforces.com/contest/2033/problem/C
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

int a[MAX_N];
int dp[MAX_N][2];

int fun(int x, int y, int xx, int yy) {
    return (x == xx) + (y == yy);
}

void solve() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }

    if (n == 2) {
        cout << (a[1] == a[2]) << endl;
        return;
    }

    int x, y;
    if ((n & 1) == 1) {
        x = a[n / 2 + 1], y = a[n / 2 + 1];
        dp[n / 2 + 1][0] = dp[n / 2 + 1][1] = 0;
    } else {
        x = a[n / 2], y = a[n / 2 + 1];
        dp[n / 2 + 1][0] = dp[n / 2 + 1][1] = (x == y);
    }

    for (int i = n / 2 + 2; i <= n; i++) {
        int xx = a[n - i + 1], yy = a[i];
        dp[i][0] = min(dp[i - 1][0] + fun(x, y, xx, yy), dp[i - 1][1] + fun(y, x, xx, yy));
        dp[i][1] = min(dp[i - 1][0] + fun(x, y, yy, xx), dp[i - 1][1] + fun(y, x, yy, xx));
        x = xx, y = yy;
    }

    cout << min(dp[n][0], dp[n][1]) << endl;
}
int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    cin >> T;
    while (T-- > 0) {
        solve();
    }

    return 0;
}
```

<br/>

## D. Kousuke's Assignment

[原题地址](https://codeforces.com/contest/2033/problem/D)

### 🏷️ 题目类型

动态规划

### 📜 题目

**题意**

在和佐仓子旅行之后，浩介非常害怕，因为他忘记了他的编程作业。在这个作业中，老师给了他一个包含 $n$ 个整数的数组 $a$，
并要求他计算数组 $a$ 中非重叠段的数量，使得每个段都被认为是美丽的。

如果一个段 $[l,r]$ 满足 $a_{l} + a_{l + 1} + ⋯ + a_{r - 1} + a_{r} = 0$ ，则该段被认为是美丽的。

对于给定的数组 $a$，您的任务是计算非重叠美丽段的最大数量。

**输入**

输入的第一行包含数字 $t$ ($1 \leq t \leq 10^{4}$)  — 测试用例的数量。每个测试用例由 $2$ 行组成。

- 第一行包含一个整数 $n$ ($1 \leq n \leq 10^{5}$)  — 数组的长度。

- 第二行包含 $n$ 个整数 $a{i}$ ($-10^{5} \leq a_{i} \leq 10^{5}$)  — 数组 $a$ 的元素。

保证所有测试用例中 $n$ 的总和不超过 $3 \cdot 10^{5}$。

**输出**

对于每个测试用例，输出一个整数：非重叠美丽段的最大数量。

**样例输入**

```text
3
5
2 1 -3 2 1
7
12 -4 4 43 -3 -5 8
6
0 -4 0 3 0 1
```

**样例输出**

```text
1
2
3
```

### 🔍 分析

**_TODO ..._**

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/29.
// https://codeforces.com/contest/2033/problem/D
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

int a[MAX_N];
long long pre[MAX_N];
long long dp[MAX_N];

void solve() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }

    for (int i = 1; i <= n; i++) {
        pre[i] = pre[i - 1] + a[i];
    }

    dp[0] = 0;

    long long ans = 0;
    map<long long, int> book;
    book[0] = 0;
    for (int i = 1; i <= n; i++) {
        if (book.find(pre[i]) != book.end()) {
            dp[i] = max(dp[i - 1], dp[book[pre[i]]] + 1);
        } else {
            dp[i] = dp[i - 1];
        }
        book[pre[i]] = i;
    }

    cout << dp[n] << endl;
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    cin >> T;
    while (T-- > 0) {
        solve();
    }

    return 0;
}
```

<br/>

## E. Sakurako, Kosuke, and the Permutation

[原题地址](https://codeforces.com/contest/2033/problem/E)

### 🏷️ 题目类型

置换环、DFS

### 📜 题目

**题意**

樱子的考试结束了，她考得非常出色。作为奖励，她得到了一个排列 $p$ 。
小介不太满意，因为他有一门考试不及格，没有收到礼物。他决定潜入她的房间（多亏了她门锁的密码）并破坏这个排列，使其变得简单。

如果对于每个 $i ~(1 \leq i \leq n)$，以下条件之一成立，则排列 $p$ 被认为是简单的：

- $p_{i}~ = ~i$

- $p_{p_{i}}~ = ~i$

例如，排列 $[1,2,3,4]$、$[5,2,4,3,1]$ 和 $[2,1]$ 是简单的，而 $[2,3,1]$ 和 $[5,2,1,4,3]$ 不是。

在一次操作中，小介可以选择索引 $i、j ~(1 \leq i，j \leq n)$ 并交换元素 $p_{i}$ 和 $p_{j}$ 。

樱子就要回家了。你的任务是计算小介需要执行的最少操作次数，以使排列变得简单。

**输入**

第一行包含一个整数 $t ~(1\leq t \leq 10^{4})$——测试用例的数量。

每个测试用例由两行描述。

- 第一行包含一个整数 $n ~(1\leq n \leq 10^{6})$——排列 $p$ 的长度。

- 第二行包含 $n$ 个整数 $p_{i} ~(1 \leq p_{i} \leq n)$——排列 $p$ 的元素。

保证所有测试用例中 $n$ 的总和不超过 $10^{6}$ 。

保证 $p$ 是一个排列。

**输出**

对于每个测试用例，输出小介需要执行的最少操作次数，以使排列变得简单。

**样例输入**

```text
6
5
1 2 3 4 5
5
5 4 3 2 1
5
2 3 4 5 1
4
2 3 4 1
3
1 3 2
7
2 3 1 5 6 7 4
```

**样例输出**

```text
0
0
2
1
0
2
```

### 🔍 分析

通过题意可以发现，两个条件都可以看作是第二个条件。而认为是简单的排列即所有环要么是自环要么是二元环。所有的排列都在环中，有的环中元素多，有的环中
元素少，在这个题中就需要把这些元素超过 $2$ 个的环全部变为元素不超过 $2$ 的环。通过观察可以得出，一个含有 $k$ 个元素的环全部变为自环或二元环最
少需要  $\frac{(k - 1)}{2}$ 次操作。排列中所有环需要的操作次数即为将排列变为简单排列的最小操作次数。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/29.
// https://codeforces.com/contest/2033/problem/E
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 1e6 + 10;

int a[MAX_N];
bool vis[MAX_N];

int dfs(int u) {
    if (vis[u]) return 0;
    vis[u] = true;
    return 1 + dfs(a[u]);
}

void solve() {
    int n;
    cin >> n;
    for (int i = 1; i <= n; i++) {
        cin >> a[i];
    }

    int ans = 0;
    fill(vis, vis + n + 1, false);
    for (int i = 1; i <= n; i++) {
        if (vis[i]) continue;
        int cnt = dfs(i);
        if (cnt > 2) ans += (cnt - 1) / 2;
    }

    cout << ans << endl;
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    cin >> T;
    while (T-- > 0) {
        solve();
    }

    return 0;
}
```

<br/>