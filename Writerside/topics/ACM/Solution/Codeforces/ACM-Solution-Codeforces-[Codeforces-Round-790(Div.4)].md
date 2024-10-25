# Codeforces Round 790 (Div. 4)

## A. Lucky?

[原题地址](https://codeforces.com/contest/1676/problem/A)

### 📜 题意

一张票是由六位数字组成的字符串。如果前三位数字的总和等于后三位数字的总和，那么这张票就被认为是幸运的。给定一张票，输出它是否幸运。请注意，票可能有前导零。

### 🔍 题解

以字符串读入 $6$ 位数字，然后直接比较前三个字符的和与后三个字符的和是否相等，因为若 ASCII 码之和相同则说明其数值之和也相同。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/11.
// https://codeforces.com/contest/1676/problem/A
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    string s;
    cin >> s;

    if (s[0] + s[1] + s[2] == s[3] + s[4] + s[5]) {
        cout << "YES" << endl;
    } else {
        cout << "NO" << endl;
    }
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

## B. Equal Candies

[原题地址](https://codeforces.com/contest/1676/problem/B)

### 📜 题意

有 $n$ 个盒子，每个盒子里糖果的数量不同。第 $i$ 个盒子里有 $a_{i}$ 颗糖果。

你还有 $n$
个朋友，你想把糖果分给他们，所以你决定给每个朋友一个盒子的糖果。但是，你不想让任何朋友不高兴，所以你决定从每个盒子里吃一些（可能没有）糖果，以使所有盒子里的糖果数量相同。请注意，你可能从不同的盒子里吃不同数量的糖果，并且你不能向任何盒子里添加糖果。

要满足要求，你必须吃的糖果总数最少是多少？

### 🔍 题解

最终需要所有盒子中的糖果数量都一样，但是每个盒子中的糖果数量只能减少，不能增加。想要吃的糖果数总数最少，则最终每个糖果的数量取决于数量最少的那一个。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/11.
// https://codeforces.com/contest/1676/problem/B
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 50 + 10;

int a[MAX_N];

void solve() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    int minVal = *min_element(a, a + n);
    int ans = 0;
    for (int i = 0; i < n; i++) {
        ans += a[i] - minVal;
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

## C. Most Similar Words

[原题地址](https://codeforces.com/contest/1676/problem/C)

### 🏷️ 题目类型

暴力

### 📜 题意

您会得到 $n$ 个长度均为 $m$ 的单词，由小写拉丁字母组成。第 $i$ 个单词表示为 $s_{i}$ 。

在一次操作中，您可以选择任何单个单词中的任何位置，并将该位置的字母更改为字母表顺序中的前一个或后一个字母。例如：

- 您可以将 `“e”` 更改为 `“d”` 或 `“f”`；

- `“a”` 只能更改为 `“b”`；

- `“z”` 只能更改为 `“y”`。

两个单词之间的差异是使它们相等所需的最少操作次数。例如，`“best”` 和 `“cost”` 之间的差异是 $1 + 10 + 0 + 0 = 11$。

找出 $s_{i}$ 和 $s_{j}$ 之间的最小差异，使得（$i \lt j$）。换句话说，找出 $n$ 个单词的所有可能对中的最小差异。

### 🔍 题解

数据范围很小，直接暴力计算即可

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/11.
// https://codeforces.com/contest/1676/problem/C
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 50 + 10;

string s[MAX_N];

int getDiff(string &s1, string s2, int m) {
    int res = 0;
    for (int i = 0; i < m; i++) {
        res += abs(s1[i] - s2[i]);
    }

    return res;
}

void solve() {
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> s[i];
    }

    int ans = INT_MAX;
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            ans = min(ans, getDiff(s[i], s[j], m));
        }
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

## D. X-Sum

[原题地址](https://codeforces.com/contest/1676/problem/D)

### 🏷️ 题目类型

暴力、枚举

### 📜 题意

铁木尔的祖父送给他一个棋盘来练习他的国际象棋技巧。这个棋盘是一个有 $n$ 行和 $m$ 列的网格，每个单元格上都写有一个非负整数。

铁木尔面临的挑战是在棋盘上放置一个象，使得象所攻击的所有单元格的总和最大。象可以沿对角线的所有方向攻击，并且象的攻击距离没有限制。请注意，放置象的单元格也被视为受到攻击。帮助他找到他能得到的最大总和。

### 🔍 题解

数据范围比较小，暴力枚举每个位置取最大值。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/11.
// https://codeforces.com/contest/1676/problem/D
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 200 + 10;

int grid[MAX_N][MAX_N];

long long getSum(int x, int y, int n, int m) {
    long long res = 0;
    int xx = x - min(x, m - y - 1), yy = y + min(x, m - y - 1);
    while (xx < n && yy >= 0) {
        res += grid[xx][yy];
        xx++, yy--;
    }

    xx = x - min(x, y), yy = y - min(x, y);
    while (xx < n && yy < m) {
        res += grid[xx][yy];
        xx++, yy++;
    }

    return res - grid[x][y];
}

void solve() {
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            cin >> grid[i][j];
        }
    }

    long long ans = 0;
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < m; j++) {
            ans = max(ans, getSum(i, j, n, m));
        }
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

## E. Eating Queries

[原题地址](https://codeforces.com/contest/1676/problem/E)

### 🏷️ 题目类型

贪心、前缀和、二分查找

### 📜 题意

铁木尔有 $n$ 颗糖果。第 $i$ 颗糖果的含糖量等于 $a_{i}$ 。所以，通过吃第 $i$ 颗糖果，铁木尔摄入的糖量等于 $a_{i}$ 。

铁木尔会向您询问有关他的糖果的 $q$ 个查询。对于第 $j$
个查询，您必须回答他需要吃的最少糖果数量，以便摄入的糖量大于或等于 $x_{j}$ ，如果无法达到这样的量则打印 $-1$
。换句话说，您应该打印出最小可能的 $k$ ，使得在吃了 $k$ 颗糖果后，铁木尔摄入的糖量至少为 $x_{j}$
，或者说不存在这样的可能 $k$。

请注意，他不能两次吃同一颗糖果，并且查询彼此独立（铁木尔可以在不同的查询中使用相同的糖果。

### 🔍 题解

在满足糖量的情况下吃的糖最少，那么应该优先吃含糖量高的糖果，所以先将糖果按含糖量从大到小排序。然后对其求前缀和，对于每次查询在前缀和数组上进行二分查找第一个大于或等于 $x_{j}$
的索引位置，即需要吃的最少糖果数，如果找不到则输出 $-1$。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/12.
// https://codeforces.com/contest/1676/problem/E
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

int a[MAX_N];
int preSum[MAX_N];

void solve() {
    int n, q;
    cin >> n >> q;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    sort(a, a + n, greater<>());

    preSum[0] = a[0];
    for (int i = 1; i < n; i++) {
        preSum[i] = preSum[i - 1] + a[i];
    }

    while (q-- > 0) {
        int x;
        cin >> x;

        int index = lower_bound(preSum, preSum + n, x) - preSum;
        if (index == n) {
            cout << -1 << endl;
        } else {
            cout << index + 1 << endl;
        }
    }
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

## F. Longest Strike

[原题地址](https://codeforces.com/contest/1676/problem/F)

### 🏷️ 题目类型

双指针

### 📜 题意

给定一个长度为 $n$ 的数组 $a$ 和一个整数 $k$ ，您的任务是找到任意两个数字 $l$ 和 $r$ （$l \leq r$），使得：

- 对于每个 $x$ （$l \leq x \leq r$）， $x$ 在 $a$ 中出现至少 $k$ 次（即 $k$ 个或更多数组元素等于 $x$）。

- 值 $r - l$ 最大。

如果没有数字满足条件，则输出 $-1$ 。

例如，如果 $a = [11, ~11, ~12, ~13, ~13, ~14, ~14]$ 且 $k = 2$，那么：

- 对于 $l = 12$， $r = 14$，第一个条件不成立，因为 $12$ 出现的次数不足 $k = 2$ 次。

- 对于 $l = 13$， $r = 14$，第一个条件成立，因为 $13$ 在 $a$ 中出现至少 $k = 2$ 次， $14$ 在 $a$ 中出现至少 $k = 2$ 次。

- 对于 $l = 11$， $r = 11$，第一个条件成立，因为 $11$ 在 $a$ 中出现至少 $k = 2$ 次。

满足第一个条件且 $r - l$ 最大的 $l$ 和 $r$ 的一对是 $l = 13$， $r = 14$。

### 🔍 题解

对数组 $a$ 先从小到大排序并去重。然后使用一个指针 $i$ 指向第一个满足 $a[i]$ 出现次数大于等于 $k$
的数，然后使用另一个指针 $j$ 开始向右移动，如果 $a[j]$ 满足 $a[j] = a[j - 1] + 1$ 且 $a[j]$ 的出现次数大于等于 $k$
则指针 $j$ 可以向右移动，否则不可以，此时 $a[i]$ 与 $a[j]$ 则是一对满足条件的结果。如此找出所有满足条件的结果取最大的一个即可。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/12.
// https://codeforces.com/contest/1676/problem/F
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    int n, k;
    cin >> n >> k;
    map<int, int> book;
    for (int i = 0, x; i < n; i++) {
        cin >> x;
        book[x]++;
    }

    vector<pair<int, int>> v(book.begin(), book.end());
    int L = 0, R = -1;
    for (int i = 0; i < v.size(); i++) {
        if (v[i].second < k) continue;

        int j = i;
        while (j + 1 < v.size() && v[j + 1].second >= k && v[j + 1].first == v[j].first + 1) j++;

        if (j - i + 1 > R - L + 1) {
            L = v[i].first;
            R = v[j].first;
        }

        i = j;
    }

    if (L == 0 && R == -1) {
        cout << -1 << endl;
    } else {
        cout << L << " " << R << endl;
    }
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

## G. White-Black Balanced Subtrees

[原题地址](https://codeforces.com/contest/1676/problem/G)

### 🏷️ 题目类型

树、DFS

### 📜 题意

给定一棵由 $n$ 个顶点组成的有根树，顶点编号从 $1$ 到 $n$。根为顶点 $1$。还有一个字符串 $s$
表示每个顶点的颜色：如果 $s[i] = $ `'B'`，则顶点 $i$ 是黑色的，如果 $s[i] = $ `'W'`，则顶点 $i$ 是白色的。

如果一棵树的子树中白色顶点的数量等于黑色顶点的数量，则称该子树是平衡的。计算平衡子树的数量。

树是一个无环的连通无向图。有根树是具有选定顶点（称为根）的树。在这个问题中，所有树的根都是 $1$。

这棵树由一个父节点数组 $a[2], ~\cdots, ~a[n]$指定，其中包含 $n - 1$ 个数字：对于所有 $i = 2, ~\cdots, ~n$，$a[i]$
是编号为 $i$ 的顶点的父节点。顶点 $u$ 的父节点是从 $u$ 到根的简单路径上的下一个顶点。

顶点 $u$ 的子树是所有在从 $u$ 到根的简单路径上经过 $u$ 的顶点的集合。例如，在下面的图中，$7$ 在 $3$
的子树中，因为简单路径 $7~→~5~→~3~→~1$ 经过 $3$。请注意，一个顶点包含在其自己的子树中，根的子树是整棵树。

![](ACM-Solution-Codeforces-CF1676G-01.png)

这张图片展示了 $n = 7$ ，$a = [1, ~1, ~2, ~3, ~3, ~5]$ 以及 $s = $ `WBBWWBW` 时的树。顶点 $3$ 处的子树是平衡的。

### 🔍 题解

一颗树中白色节点的数量：

- $白色节点的数量 = 根节点白色节点的数量 + 子树白色节点的数量$

- $黑色节点的数量 = 根节点黑色节点的数量 + 子树黑色节点的数量$

使用 DFS 实现此过程即可。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/12.
// https://codeforces.com/contest/1676/problem/G
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

vector<int> edge[MAX_N];
string s;

struct TreeColor {
    int white;
    int black;
};

void init(int n) {
    for (int i = 0; i <= n; i++) {
        edge[i].clear();
    }
}

TreeColor dfs(int root, long long &ans) {
    TreeColor treeColor{0, 0};
    if (s[root - 1] == 'W') treeColor.white++;
    if (s[root - 1] == 'B') treeColor.black++;

    for (int to: edge[root]) {
        TreeColor subTreeColor = dfs(to, ans);
        treeColor.white += subTreeColor.white;
        treeColor.black += subTreeColor.black;
    }

    if (treeColor.white == treeColor.black) {
        ans++;
    }

    return treeColor;
}

void solve() {
    int n;
    cin >> n;

    init(n);
    for (int i = 2, vertex; i <= n; i++) {
        cin >> vertex;
        edge[vertex].push_back(i);
    }

    cin >> s;

    long long ans = 0;
    dfs(1, ans);

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

## H1. Maximum Crossings (Easy Version)

[原题地址](https://codeforces.com/contest/1676/problem/H1)

### 🏷️ 题目类型

暴力、树状数组、线段树

### 📜 题意

这两个版本之间的唯一区别在于，在这个版本中，$n \leq 1000$ 并且所有测试用例中 $n$ 的总和不超过 $1000$。

一个终端是一排按顺序编号为 $1$ 到 $n$ 的 $n$ 个相等的段。有两个终端，一个在另一个上面。

给您一个长度为 $n$ 的数组 $a$。对于所有 $i ~= ~1, ~2, ~\cdots, ~n$，应该有一条直线从顶部终端的第 $i$
段上的某个点到底部终端的第 $a_{i}$ 段上的某个点。您不能选择段的端点。例如，如果 $n = 7$ 且 $a = [4, 1, 4, 6, 7, 7, 5]$
，以下图片展示了两种可能的布线。

![](ACM-Solution-Codeforces-CF1676H1-01.png)

当两根电线有一个共同的点时，就会出现交叉。在上面的图片中，交叉点用红色圆圈圈出。

如果您以最佳方式放置电线，最多可能会有多少个交叉点？

### 🔍 题解

假设 $[a, ~b]$ 表示一根一边在第一个终端第 $a$ 段，另一边在第二个终端第 $b$ 段的线。那么对于两根线 $[a, ~b]$、$[c, ~d]$
其中 $a < b$，想要 这两条线有交点，那么一定有 $b \geq d$。

所以对于 $a$ 中第 $i$ 个线可以与前 $[1, i)$ 个线产生交点的：$a[j] \geq a[i]~(1 \leq j \leq i)$。该题数据规模比较小可以直接暴力计算。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/13.
// https://codeforces.com/contest/1676/problem/H1
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

int a[MAX_N];

struct BIT {
    long long *C;
    int N;

    BIT(int n) {
        N = n;
        C = (long long *) malloc(sizeof(long long) * N);
        memset(C, 0, sizeof(long long) * N);
    }

    void add(int index, int value) {
        while (index < N) {
            C[index] += value;
            index += lowbit(index);
        }
    }

    int query(int index) {
        int res = 0;
        while (index > 0) {
            res += C[index];
            index -= lowbit(index);
        }
        return res;
    }

    int lowbit(int x) {
        return x & (-x);
    }
};

void solve() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    BIT bit(n + 1);
    long long ans = 0;
    for (int i = 0; i < n; i++) {
        ans += i - bit.query(a[i] - 1);
        bit.add(a[i], 1);
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

## H2. Maximum Crossings (Hard Version)

[原题地址](https://codeforces.com/contest/1676/problem/H2)

### 🏷️ 题目类型

树状数组、线段树

### 📜 题意

这两个版本之间的唯一区别在于，在这个版本中，$n \leq 2 \cdot 10^{5}$ 并且所有测试用例中 $n$ 的总和不超过 $2 \cdot 10^{5}$。

一个终端是一排按顺序编号为 $1$ 到 $n$ 的 $n$ 个相等的段。有两个终端，一个在另一个上面。

给您一个长度为 $n$ 的数组 $a$。对于所有 $i ~= ~1, ~2, ~\cdots, ~n$，应该有一条直线从顶部终端的第 $i$
段上的某个点到底部终端的第 $a_{i}$ 段上的某个点。您不能选择段的端点。例如，如果 $n = 7$ 且 $a = [4, 1, 4, 6, 7, 7, 5]$
，以下图片展示了两种可能的布线。

![](ACM-Solution-Codeforces-CF1676H1-01.png)

当两根电线有一个共同的点时，就会出现交叉。在上面的图片中，交叉点用红色圆圈圈出。

如果您以最佳方式放置电线，最多可能会有多少个交叉点？

### 🔍 题解

假设 $[a, ~b]$ 表示一根一边在第一个终端第 $a$ 段，另一边在第二个终端第 $b$ 段的线。那么对于两根线 $[a, ~b]$、$[c, ~d]$
其中 $a < b$，想要 这两条线有交点，那么一定有 $b \geq d$。

所以对于 $a$ 中第 $i$ 个线可以与前 $[1, i)$ 个线产生交点的：$a[j] \geq a[i]~(1 \leq j \leq i)$。可以使用树状数组或线段树来统计。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/13.
// https://codeforces.com/contest/1676/problem/H2
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

int a[MAX_N];

struct BIT {
    long long *C;
    int N;

    BIT(int n) {
        N = n;
        C = (long long *) malloc(sizeof(long long) * N);
        memset(C, 0, sizeof(long long) * N);
    }

    void add(int index, int value) {
        while (index < N) {
            C[index] += value;
            index += lowbit(index);
        }
    }

    int query(int index) {
        int res = 0;
        while (index > 0) {
            res += C[index];
            index -= lowbit(index);
        }
        return res;
    }

    int lowbit(int x) {
        return x & (-x);
    }
};

void solve() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    BIT bit(n + 1);
    long long ans = 0;
    for (int i = 0; i < n; i++) {
        ans += i - bit.query(a[i] - 1);
        bit.add(a[i], 1);
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