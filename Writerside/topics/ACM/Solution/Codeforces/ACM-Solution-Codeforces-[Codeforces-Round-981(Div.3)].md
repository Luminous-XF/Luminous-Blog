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

## F. Kosuke's Sloth

[原题地址](https://codeforces.com/contest/2033/problem/F)

### 🏷️ 题目类型

斐波那契数列、数论、打表

### 📜 题目

**题意**

小介太懒了。他不会给您任何传说，只有任务：

斐波那契数定义如下：

- $f(1)=f(2)=1$。

- $f(n)=f(n−1)+f(n−2)$ （$3≤n$）

我们将 $G(n,k)$ 表示为第 $n$ 个可被 $k$ 整除的斐波那契数的索引。对于给定的 $n$ 和 $k$，计算 $G(n,k)$。

由于这个数字可能太大，将其对 $10^{9} + 7$ 取模后输出。

例如：$G(3,2)=9$ 因为第 $3$ 个可被 $2$ 整除的斐波那契数是 $34$ 。$[1,~1,~2,~3,~5,~8,~13,~21,~34]$ 。

**输入**

输入数据的第一行包含一个整数 $t$ ($1 \leq t \leq 10^{4}$) — 测试用例的数量。

第一行也是唯一一行包含两个整数 $n$ 和 $k$ ($1 \leq n \leq 10^{18}, 1 \leq k \leq 10^{5}$) 。

保证所有测试用例中 $k$ 的总和不超过 $10^{6}$ 。

**输出**

对于每个测试用例，输出唯一的数字：$G(n,k)$ 对 $10^{9} + 7$ 取模的值。

**样例输入**

```text
3
3 2
100 1
1000000000000 1377
```

**样例输出**

```text
9
100
999244007
```

### 🔍 分析

通过打表可以发现数列中可以被 $k$ 整除的数是周期出现的，这个是[皮萨诺周期（Pisano Period）](https://en.m.wikipedia.org/wiki/Pisano_period)。
所以可以暴力找到第一个可以被 $k$ 整除的斐波那契数所在的位置 $index$，则第 $n$ 个可以被 $k$ 整除的数所在的位置为 $p ~\cdot n$。

### 💡 代码

时间复杂度：$O(k)$

```C++
//
// Created by Luminous on 2024/10/30.
// https://codeforces.com/contest/2033/problem/F
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 1e6 + 10;
const long long MOD = 1e9 + 7;

long long f[MAX_N];

void solve() {
    long long n, k;
    cin >> n >> k;

    f[1] = f[2] = 1;

    int index = 0;
    for (int i = 1; ; i++) {
        f[i] = (i > 2 ? f[i - 2] + f[i - 1] : f[i]) % k;
        if (f[i] % k == 0) {
            index = i;
            break;
        }
    }

    cout << index % MOD * (n % MOD) % MOD << endl;
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

## G. Sakurako and Chefir

[原题地址](https://codeforces.com/contest/2033/problem/G)

### 🏷️ 题目类型

树、线段树、贪心

### 📜 题目

**题意**

给定一棵以顶点 $1$ 为根的有 $n$ 个顶点的树。当樱子和她的猫谢菲尔一起在树中散步时，樱子分心了，谢菲尔跑掉了。

为了帮助樱子，小介记录了他的 $q$ 个猜测。在第 $i$ 次猜测中，他假设谢菲尔在顶点 $v_{i}$ 迷路了，并且有 $k_{i}$ 点耐力。

此外，对于每个猜测，小介假设谢菲尔可以沿着边任意次数移动：

- 从顶点 $a$ 到顶点 $b$，如果 $a$ 是 $b$ 的祖先∗，耐力不会改变；

- 从顶点 $a$ 到顶点 $b$，如果 $a$ 不是 $b$ 的祖先，那么谢菲尔的耐力减少 $1$ 。

如果谢菲尔的耐力为 $0$ ，他就不能进行第二种类型的移动。

对于每个假设，您的任务是找到从顶点 $v_{i}$ 出发，拥有 $k_{i}$ 点耐力的情况下，谢菲尔能够到达的最远顶点的距离。

**∗** 如果从 $b$ 到根的最短路径经过 $a$ ，则顶点 $a$ 是顶点 $b$ 的祖先。

**输入**

第一行包含一个整数 $t$ （$1 \leq t \leq 10^{4}$）  — 测试用例的数量。

每个测试用例的描述如下：

- 第一行包含一个整数 $n$ （$2 \leq n \leq 2 \cdot 10^{5}$）  — 树中的顶点数量。

- 接下来的 $n - 1$ 行包含树的边。保证给定的边构成一棵树。

- 下一行由一个整数 $q$ （$1 \cdot q \cdot 2 \cdot 10^{5}$）组成，它表示小介做出的猜测数量。

- 接下来的 $q$ 行描述了小介做出的猜测，有两个整数 $v_{i}$ 、 $k_{i}$ （$1 \leq v_{i} \leq n$，$0 \leq k_{i} \leq n$）。

保证所有测试用例中 $n$ 的总和以及 $q$ 的总和不超过 $2 \cdot 10^{5}$ 。

**输出**

对于每个测试用例和每次猜测，输出从起始点 $v_{i}$ 出发，在拥有 $k_{i}$ 耐力的情况下，Chefir 能够到达的最远顶点的最大距离。

**样例输入**

```text
3
5
1 2
2 3
3 4
3 5
3
5 1
3 1
2 0
9
8 1
1 7
1 4
7 3
4 9
3 2
1 5
3 6
7
6 0
2 3
6 2
8 2
2 4
9 2
6 3
6
2 1
2 5
2 4
5 6
4 3
3
3 1
1 3
6 5
```

**样例输出**

```text
2 1 2 
0 5 2 4 5 5 5 
1 3 4 
```

### 🔍 分析

根据题意可以知道在树上有两种移动方式： 

- 向下移动（不消耗体力）
- 向上移动（每移动一步消耗一点体力）

下图是从节点 $3$ 向下移动到节点 $15$，移动路径：$3 \rightarrow 6 \rightarrow 10 \rightarrow 15$

![ACM-Solution-Codeforces-CF2033G-01.png](ACM-Solution-Codeforces-CF2033G-01.png)

下图是从节点 $3$ 向上移动到节点 $1$，移动路径：$3 \rightarrow 1$

![ACM-Solution-Codeforces-CF2033G-02.png](ACM-Solution-Codeforces-CF2033G-02.png)


因为向下移动不消耗体力，所以一定可以从当前节点移动到该节点所在子树的最深的叶节点。也可以使用体力向上移动后再往下移动。

那么对于节点 $10$ 来说就有以下移动路径

向下移动：

- $10 \rightarrow 15$
- $10 \rightarrow 14$

向上后再向下移动：

- $10  \rightarrow  6  \rightarrow 9 \rightarrow 13$
- $10  \rightarrow  6  \rightarrow 3 \rightarrow 5 \rightarrow 8 \rightarrow 12 \rightarrow 17$
- $10  \rightarrow  6  \rightarrow 3 \rightarrow 1 \rightarrow 2 \rightarrow 4 \rightarrow 7 \rightarrow 11 \rightarrow 16$

![ACM-Solution-Codeforces-CF2033G-03.png](ACM-Solution-Codeforces-CF2033G-03.png)

而想要起始节点和最后到达节点相距最远，就需要选以上路径中移动最远的那个，即 $15 \rightarrow 16$ 这条路径。

将问题一般化，假设 $maxDepth[u]$ 表示节点 $u$ 可以向下移动的最大距离，
那么从节点 $u$ 向上移动 $i$ 个节点可以到达的最大距离即 $i + maxDepth[u + i]$。
若有 $k$ 点体力，那么从节点 $u$ 出发可以移动的最远距离为：
$max\{maxDepth[u], ~ 1 + maxDepth[u + 1], ~ 2 + maxDepth[u + 2],  ~\cdots,~ k + maxDepth[u + k] \}$

若每次枚举向上的 $k$ 个节点，那么单次查询的最坏时间复杂度为 $O(k)$，无法接受。接下来考虑如何优化，假设 $depth[u]$ 表示当前节点所在的深度，
那么我们每次枚举的都是一条链上 $depth[u] ~ depth[u + k]$ 之间的点的 $maxDepth$。此时发现在同一条链上的所有查询，所枚举的点的范围是存在重叠的，
那么我们可以先将查询离线出来，对树上的每条链构建线段树，维护区间的最大 $maxDepth$，然后 DFS 遍历所有点，若该节点上存在查询，则在线段树中进行查询。
在遍历完一条链后，在回溯时可以重置一下当前节点在线段树中的值，这样就只需要一颗线段树。这样对于单次查询时间复杂度只需要 $O(\log{n})$。

PS:

![ACM-Solution-Codeforces-CF2033G-04.png](ACM-Solution-Codeforces-CF2033G-04.png)

从节点 $10$ 向上走到节点 $3$ 后再向下走到 节点 $17$ 的距离为：

$depth[10] + maxDepth[3] - depth[3]$

将这个一般化，假设从节点 $s$ 向上走到节点 $u$ 再向下走到节点 $e$ 的距离为

$depth[s] + maxDepth[u] - depth[u]$

所以在线段树中维护的是 $maxDepth[u] - depth[u]$ 的最大值！


### 💡 代码

```C++
//
// Created by Luminous on 2024/10/31.
// https://codeforces.com/contest/2033/problem/G
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

struct Query {
    int k;
    int id;
};

struct SegmentTree {
    int n;
    int *C;

    explicit SegmentTree(int n) {
        this->n = (n + 10) << 2;
        C = (int *) malloc(sizeof(int) * this->n);
    }

    void pushUp(int root) {
        C[root] = max(C[root << 1], C[root << 1 | 1]);
    }

    void build(int l, int r, int root) {
        if (l == r) {
            C[root] = INT_MIN;
        } else {
            int mid = l + r >> 1;
            build(l, mid, root << 1);
            build(mid + 1, r, root << 1 | 1);
            pushUp(root);
        }
    }

    void update(int k, int v, int l, int r, int root) {
        if (l == r) {
            C[root] = v;
            return;
        }

        int mid = l + r >> 1;
        if (k <= mid) {
            update(k, v, l, mid, root << 1);
        } else {
            update(k, v, mid + 1, r, root << 1 | 1);
        }

        pushUp(root);
    }

    int query(int L, int R, int l, int r, int root) {
        if (L <= l && r <= R) {
            return C[root];
        }

        int res = INT_MIN;
        int mid = l + r >> 1;
        if (L <= mid) {
            res = max(res, query(L, R, l, mid, root << 1));
        }
        if (mid < R) {
            res = max(res, query(L, R, mid + 1, r, root << 1 | 1));
        }

        return res;
    }
};

vector<int> edge[MAX_N];
int depth[MAX_N];
int maxDepth[MAX_N];
vector<Query> query[MAX_N];
int ans[MAX_N];

void init(int n) {
    for (int i = 0; i < n + 10; i++) {
        edge[i].clear();
        depth[i] = 0;
        maxDepth[i] = 0;
        query[i].clear();
    }
}

void getDepth(int u, int fa) {
    for (int v: edge[u]) {
        if (v == fa) continue;
        depth[v] = depth[u] + 1;
        getDepth(v, u);
        maxDepth[u] = max(maxDepth[u], maxDepth[v] + 1);
    }
}

void dfs(int u, int fa, int n, SegmentTree &segTree) {
    for (auto &[k, id]: query[u]) {
        int dis = segTree.query(max(0, depth[u] - k), depth[u], 0, n - 1, 1) + depth[u];
        ans[id] = max(maxDepth[u], dis);
    }

    int max1 = 0, max2 = 0;
    for (int v : edge[u]) {
        if (v == fa) continue;

        if (maxDepth[v] + 1 > max1) {
            max2 = max1;
            max1 = maxDepth[v] + 1;
        } else if (maxDepth[v] + 1 > max2) {
            max2 = maxDepth[v] + 1;
        }
    }

    for (int v: edge[u]) {
        if (v == fa) continue;

        int d = maxDepth[v] + 1 == max1 ? max2 : max1;
        segTree.update(depth[u], d - depth[u], 0, n - 1, 1);

        dfs(v, u, n, segTree);
        segTree.update(depth[u], INT_MIN, 0, n - 1, 1);
    }
}

void solve() {
    int n;
    cin >> n;

    init(n);

    for (int i = 0; i < n - 1; i++) {
        int u, v;
        cin >> u >> v;
        edge[u].push_back(v);
        edge[v].push_back(u);
    }

    int q;
    cin >> q;
    for (int i = 0; i < q; i++) {
        int v, k;
        cin >> v >> k;
        query[v].push_back({k, i});
    }

    getDepth(1, 0);

    SegmentTree segTree(n);
    segTree.build(0, n - 1, 1);

    dfs(1, 0, n, segTree);

    for (int i = 0; i < q; i++) {
        cout << ans[i] << " ";
    }
    cout << endl;
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