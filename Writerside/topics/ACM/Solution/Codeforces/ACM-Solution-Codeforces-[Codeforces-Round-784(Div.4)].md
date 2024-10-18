# Codeforces Round 784 (Div. 4)

## A. Division?

[原题地址](https://codeforces.com/contest/1669/problem/A)

### 📜 题意

Codeforces 根据用户的评级将其分为 $4$ 个分区：

- 对于第 $1$ 分区：$1900 \leq rating$

- 对于第 $2$ 分区：$1600 \leq rating \leq 1899$

- 对于第 $3$ 分区：$1400 \leq rating \leq 1599$

- 对于第 $4$ 分区：$rating \leq 1399$

给定一个评级，输出该评级所属的分区。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/8.
// https://codeforces.com/contest/1669/problem/A
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    int score;
    cin >> score;

    if (score < 1400) {
        cout << "Division 4" << endl;
    } else if (score < 1600) {
        cout << "Division 3" << endl;
    } else if (score < 1900) {
        cout << "Division 2" << endl;
    } else {
        cout << "Division 1" << endl;
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

## B. Triple

[原题地址](https://codeforces.com/contest/1669/problem/B)

### 📜 题意

给定一个包含 $n$ 个元素的数组 $a$ ，打印任何出现至少三次的值，若没有这样的值则打印 $-1$ 。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/8.
// https://codeforces.com/contest/1669/problem/B
//

#pragma GCC optimize(3)
#include <bits/stdc++.h>

using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

unordered_map<int, int> book;

void init() {
    book.clear();
}

void solve() {
    int n;
    cin >> n;

    for (int i = 0, x; i < n; i++) {
        cin >> x;
        book[x]++;
    }

    for (auto &[num, cnt] : book) {
        if (cnt >= 3) {
            cout << num << endl;
            return;
        }
    }

    cout << -1 << endl;
}

int main() {

    ios::sync_with_stdio(false);

    int T = 1;
    cin >> T;
    while (T-- > 0) {
        init();
        solve();
    }

    return 0;
}
```

<br/>

## C. Odd/Even Increments

[原题地址](https://codeforces.com/contest/1669/problem/C)

### 📜 题意

给定一个由 $n$ 个正整数组成的数组 $a = [a_{1}, ~a_{2}, ~\cdots, ~a_{n}]$ ，您可以对其执行两种类型的操作：

1. 对每个奇数索引的元素加 1
   。换句话说，按如下方式更改数组：$a_{1} := a_{1} + 1$ ，$a_{3} := a_{3} + 1$ ，$a_{5} := a_{5} + 1$ ，$\cdots \cdots$

2. 对每个偶数索引的元素加 1
   。换句话说，按如下方式更改数组：$a_{2} := a_{2} + 1$ ，$a_{4} := a_{4} + 1$ ，$a_{6} := a_{6} + 1$ ，$\cdots \cdots$

确定在进行任意数量的操作后，最终数组是否可能只包含偶数或只包含奇数。换句话说，确定在进行任意数量的操作后，您是否可以使数组的所有元素具有相同的奇偶性。

请注意，您可以对两种类型的操作执行任意次数（甚至不执行）。不同类型的操作可以执行不同的次数。

### 🔍 题解

- $奇数 + 1 = 偶数$
-
- $偶数 + 1 = 奇数$

想要对所有奇数或偶数的位置上的数同时加 $1$ 后的结果同为奇数或偶数，
那么就需要满足：

- 所有奇数位置上的数同为奇数或同为偶数
- 所有偶数位置上的数同为奇数或同为偶数

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/8.
// https://codeforces.com/contest/1669/problem/C
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    int n;
    cin >> n;

    int oddCnt0 = 0, oddCnt1 = 0, evenCnt0 = 0, evenCnt1 = 0;
    for (int i = 1, x; i <= n; i++) {
        cin >> x;
        if ((i & 1) == 1) {
            if ((x & 1) == 1) {
                oddCnt1++;
            } else {
                oddCnt0++;
            }
        } else {
            if ((x & 1) == 1) {
                evenCnt1++;
            } else {
                evenCnt0++;
            }
        }
    }

    if ((oddCnt0 == (n + 1) / 2 || oddCnt1 == (n + 1) / 2) &&
        (evenCnt0 == n / 2 || evenCnt1 == n / 2)) {
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

## D. Colorful Stamp

[原题地址](https://codeforces.com/contest/1669/problem/D)

### 🏷️ 题目类型

思维

### 📜 题意

给定一排 $n$ 个单元格，最初都是白色的。使用一个印章，您可以在任意两个相邻的单元格上盖章，使得一个变为红色，另一个变为蓝色。印章可以旋转，即可以以两种方式使用：作为
`BR` 和作为 `RB`。

在使用过程中，印章必须完全适合给定的 $n$ 个单元格（不能部分在单元格之外）。印章可以多次应用于同一个单元格。每次使用印章都会重新为印章下的两个单元格着色。

例如，制作图案 `BRBBW` 的一种可能的盖章序列可以是 `WWWWW` → `WWRB–––W` → `BR–––RBW` → `BRB–––BW` 。这里 `W`、`R` 和 `B`
分别代表白色、红色或蓝色的单元格，印章使用的单元格用下划线标记。

给定最终的图案，是否可以使用印章零次或多次来制作它？

### 🔍 题解

只有一种情况无法绘制出：一段连续的颜色相同的单元格。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/9.
// https://codeforces.com/contest/1669/problem/D
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

bool check(string s) {
    return count(s.begin(), s.end(), s[0]) != s.length();
}

void solve() {
    int n;
    string s;
    cin >> n >> s;

    for (int i = 0; i < s.length(); i++) {
        if (s[i] == 'W') continue;

        int j = i;
        while (j + 1 < s.length() && s[j + 1] != 'W') j++;

        if (!check(s.substr(i, j - i + 1))) {
            cout << "NO" << endl;
            return;
        }

        i = j;
    }

    cout << "YES" << endl;
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

## E. 2-Letter Strings

[原题地址](https://codeforces.com/contest/1669/problem/E)

### 🏷️ 题目类型

枚举贡献

### 📜 题意

给定 $n$ 个长度为 $2$ 的字符串，由小写拉丁字母从 `'a'` 到 `'k'` 组成，输出索引对 $(i,~j)$ 的数量，其中 $i \lt j$ 且第 $i$
个字符串和第 $j$ 个字符串在恰好一个位置上不同。

换句话说，计算对数 $(i,~j)$（$i \lt j$），使得第 $i$ 个字符串和第 $j$ 个字符串有恰好一个位置 $p$（$1 \leq p \leq 2$
），使得 $s_{ip} \neq s_{jp}$ 。

### 🔍 题解

- 使用 `book1[i][j]` 记录以 `i` 开头 `j` 结尾的字符串
-
- 使用 `book2[i][j]` 记录以 `i` 结尾 `i` 开头的字符串

那么前 $i - 1$ 个字符串中与第 $i$ 个字符串 $s_{i}$ 满足恰好只有一个字符不同的字符串的个数为：

$\sum_{c = 'a'}^{'k'} book1[s_{i0}][c] ~ \mathrm{(c \neq s_{i1})} + \sum_{c = 'a'}^{'k'} book2[s_{i1}][c] ~ \mathrm{(c \neq s_{i0})}$

其中 book1 与 book2 只统计 $i$ 之前的。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/9.
// https://codeforces.com/contest/1669/problem/E
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"

const int N = 11;
const int MAX_N = 1e5 + 10;

string str[MAX_N];

void solve() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> str[i];
    }

    long long ans = 0;
    int book1[N][N] = {0}, book2[N][N] = {0};
    for (int i = 0; i < n; i++) {
        int c1 = str[i][0] - 'a', c2 = str[i][1] - 'a';
        for (int c = 0; c < N; c++) {
            if (c2 != c) ans += book1[c1][c];
            if (c1 != c) ans += book2[c2][c];
        }

        book1[c1][c2]++;
        book2[c2][c1]++;
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

## F. Eating Candies

[原题地址](https://codeforces.com/contest/1669/problem/F)

### 🏷️ 题目类型

双指针、二分

### 📜 题意

有 $n$ 颗糖果从左到右放在一张桌子上。这些糖果从左到右编号。第 $i$ 颗糖果的重量为 $w_{i}$ 。爱丽丝和鲍勃吃糖果。

爱丽丝可以从左边吃任意数量的糖果（她不能跳过糖果，她按顺序吃）。

鲍勃可以从右边吃任意数量的糖果（他不能跳过糖果，他按顺序吃）。

当然，如果爱丽丝吃了一颗糖果，鲍勃就不能吃它（反之亦然）。

他们想要公平。他们的目标是吃到相同总重量的糖果。他们总共能吃到的最多糖果数量是多少？

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/10.
// https://codeforces.com/contest/1669/problem/F
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

int a[MAX_N];

void solve() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    int ans = 0, leftSum = 0, rightSum = 0;
    for (int i = -1, j = n; i < j;) {
        if (leftSum == rightSum) {
            ans = i + 1 + n - j;
            leftSum += a[++i];
        } else {
            if (j - i == 1) break;
            if (leftSum + a[i + 1] <= rightSum) {
                leftSum += a[++i];
            } else {
                rightSum += a[--j];
            }
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

## G. Fall Down

[原题地址](https://codeforces.com/contest/1669/problem/G)

### 🏷️ 题目类型

模拟

### 📜 题意

有一个 $n$ 行 $m$ 列的网格，以及三种类型的单元格：

- 空单元格，用 `'.'` 表示。

- 石头，用 `'*'` 表示。

- 障碍物，用小写拉丁字母 `'o'` 表示。

所有的石头都会下落，直到它们碰到地板（底部那一行）、障碍物或者其他已经不能移动的石头。（换句话说，只要能下落，所有的石头都会一直下落。）

模拟这个过程。最终得到的网格是什么样子？

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/10.
// https://codeforces.com/contest/1669/problem/G
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 50 + 10;

string grid[MAX_N];

void fallDown(int column, int n) {
    for (int i = n - 1, bound = n; i >= 0; i--) {
        if (grid[i][column] == '*') {
            swap(grid[bound - 1][column], grid[i][column]);
            bound--;
        } else if (grid[i][column] == 'o') {
            bound = i;
        }
    }
}

void solve() {
    int n, m;
    cin >> n >> m;
    for (int i = 0; i < n; i++) {
        cin >> grid[i];
    }

    for (int i = 0; i < m; i++) {
        fallDown(i, n);
    }

    for (int i = 0; i < n; i++) {
        cout << grid[i] << endl;
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

## H. Maximal AND

[原题地址](https://codeforces.com/contest/1669/problem/H)

### 🏷️ 题目类型

贪心、位运算

### 📜 题意

令 `AND` 表示[按位与运算](https://en.wikipedia.org/wiki/Bitwise_operation#AND) ，`OR`
表示[按位或运算](https://en.wikipedia.org/wiki/Bitwise_operation#OR) 。

给定一个长度为 $n$ 的数组 $a$ 和一个非负整数 $k$ 。您可以对数组执行最多 $k$ 次以下类型的操作：

- 选择一个索引 $i$ （$1 \leq i \leq n$），并用 $a_{i} ~ OR ~ 2^{j}$ 替换 $a_{i}$ ，其中 $j$ 是介于 $0$ 到 $30$（包括 $0$
  和 $30$）之间的任何整数。换句话说，在一次操作中，您可以选择一个索引 $i$ （$1 \leq i \leq n$）并将 $a_{i}$ 的第 $j$
  位设置为 $1$ （$0 \leq j \leq 30$ ）。

输出执行最多 $k$ 次操作后 $a_{1} ~ AND ~ a_{2} ~ AND ~ \cdots ~AND ~ a_{n}$ 的最大可能值。

### 🔍 题解

- $0 ~ AND ~ 0 ~ = ~0$

- $1 ~ AND ~ 0 ~ = ~0$

- $1 ~ AND ~ 1 ~ = ~0$

所以当所有数的第 $i$ 位都为 $1$ 时最终的结果的第 $i$ 位才能为
1。现在想要最终的结果最大，那么可以贪心的先将所有数的高位都变为 $1$，直到 $k$
次操作机会都使用完或所有数的每一位都已经为 $1$。

### 💡 代码

```C++
//
// Created by Luminous on 2024/10/10.
// https://codeforces.com/contest/1669/problem/H
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

int a[MAX_N];

void solve() {
    int n, k;
    cin >> n >> k;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    int cnt[31] = {0};
    for (int i = 0; i < n; i++) {
        for (int j = 0; j < 31; j++) {
            cnt[j] += (a[i] >> j) & 1;
        }
    }

    int ans = 0;
    for (int i = 30; i >= 0; i--) {
        if (n - cnt[i] <= k) {
            k -= n - cnt[i];
            ans += 1 << i;
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