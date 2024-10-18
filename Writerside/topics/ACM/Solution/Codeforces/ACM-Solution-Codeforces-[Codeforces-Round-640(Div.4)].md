# Codeforces-Round 640 (Div.4)

## A. Sum of Round Numbers

[原题地址](https://codeforces.com/contest/1352/problem/A)

### 题意

一个正整数如果是`d00...0`的形式就被称为“圆整”的。换句话说，如果一个正整数除了最左边（最高位）的数字外，其余所有数字都等于零，那么这个正整数就是圆整的。
特别地，从 1 到 9（包括 1 和 9）的所有数字都是圆整的。

例如，以下数字是圆整的：$4000、1、9、800、90$。以下数字不是圆整的：$110、707、222、1001$。

给您一个正整数 $n$（$1 \leq n \leq 10^4$）。用最少的加数将数字 $n$ 表示为圆整数字的和。换句话说，您需要将给定的数字 $n$
表示为最少数量的项的和，其中每一项都是圆整数字。

### 代码

```C++
//
// Created by Luminous on 2024/9/30.
// https://codeforces.com/contest/1352/problem/A
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    int n;
    cin >> n;

    vector<int> ans;
    int v = 1;
    while (n > 0) {
        int x = n % 10 * v;
        if (x != 0) {
            ans.push_back(x);
        }
        v *= 10;
        n /= 10;
    }

    cout << ans.size() << endl;
    for (int x : ans) {
        cout << x << " ";
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

## B. Same Parity Summands

[原题地址](https://codeforces.com/contest/1352/problem/B)

### 题意

给定两个正整数 $n$（$1 \leq n \leq 10^9$）和 $k$（$1 \leq k \leq 100$）。将数字 $n$ 表示为 $k$ 个具有相同奇偶性（除以 $2$
时余数相同）的正整数的和。

换句话说，找到 $a_{1}, ~a_{2}, ..., ~a_{k}$ 使得所有 $a_{i} \gt 0$，$n ~ = ~ a_{1} + a_{2} + ... + a_{k}$，
并且要么所有 $a_{i}$ 都是偶数，要么所有 $a_{i}$ 都是奇数。

如果这样的表示不存在，则进行报告。

### 代码

```C++
//
// Created by Luminous on 2024/9/30.
// https://codeforces.com/contest/1352/problem/B
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    int n, k;
    cin >> n >> k;

    if (k > n) {
        cout << "NO" << endl;
        return;
    }

    if (n % 2 == 1) {
        if ((n - k) % 2 == 0) {
            cout << "YES" << endl;
            for (int i = 0; i < k - 1; i++) {
                cout << 1 << " ";
            }
            cout << n - (k - 1) << endl;
        } else {
            cout << "NO" << endl;
        }
    } else {
        if (n >= 2 * k) {
            cout << "YES" << endl;
            for (int i = 0; i < k - 1; i++) {
                cout << 2 << " ";
            }
            cout << n - 2 * (k - 1) << endl;
        } else {
            if (k % 2 == 0) {
                cout << "YES" << endl;
                for (int i = 0; i < k - 1; i++) {
                    cout << 1 << " ";
                }
                cout << n - (k - 1) << endl;
            } else {
                cout << "NO" << endl;
            }
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

## C. K-th Not Divisible by n

[原题地址](https://codeforces.com/contest/1352/problem/C)

### 题意

您将获得两个正整数 $n$ 和 $k$。打印第 $k$ 个不能被 $n$ 整除的正整数。

例如，如果 $n = 3$ ， $k = 7$ ，那么所有不能被 $3$ 整除的数是：$1、2、4、5、7、8、10、11、13 ...$ 其中第 $7$ 个数是 $10$。

### 代码

```C++
//
// Created by Luminous on 2024/9/30.
// https://codeforces.com/contest/1352/problem/C
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    int n, k;
    cin >> n >> k;

    int x = k % (n - 1);
    cout << k / (n - 1) * n + (x == 0 ? - 1 : x) << endl;
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

## D. Alice, Bob and Candies

[原题地址](https://codeforces.com/contest/1352/problem/D)

### 题目类型

模拟

### 题意

有 $n$ 颗糖果排成一排，从左到右编号为 $1$ 到 $n$。第 $i$ 颗糖果的大小为 $a_{i}$。

爱丽丝和鲍勃玩一个有趣又美味的游戏：他们吃糖果。爱丽丝从左往右吃，鲍勃从右往左吃。当所有糖果都被吃完时游戏结束。

这个过程有若干步。在每一步中，玩家从自己的那一侧吃一颗或多颗糖果（爱丽丝从左边吃，鲍勃从右边吃）。

爱丽丝先走第一步。在第一步，她会吃 $1$ 颗糖果（其大小为 $a_{1}$）。然后，在接下来的每一步中，玩家交替进行——也就是说，鲍勃走第二步，然后是爱丽丝，接着又是鲍勃，以此类推。

在每一步中，玩家计算当前步所吃糖果的总大小。一旦这个数字严格大于另一个玩家在上一步所吃糖果的总大小，当前玩家就停止进食，这一步结束。换句话说，在每一步中，玩家吃尽可能少的糖果，使得这一步所吃糖果的大小总和严格大于另一个玩家在上一步所吃糖果的大小总和。如果没有足够的糖果按照这种方式进行一步，那么玩家就吃掉所有剩余的糖果，游戏结束。

例如，如果 $n = 11$ 且 $a = [3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5]$ ，那么：

- 第一步：爱丽丝吃了一颗大小为 $3$ 的糖果，糖果序列变为 $[1, 4, 1, 5, 9, 2, 6, 5, 3, 5]$ 。

- 第二步：爱丽丝在上一步吃了 $3$，这意味着鲍勃必须吃 $4$ 或更多。鲍勃吃了一颗大小为 $5$
  的糖果，糖果序列变为 $[1, 4, 1, 5, 9, 2, 6, 5, 3]$。

- 第三步：鲍勃在上一步吃了 $5$，这意味着爱丽丝必须吃 $6$ 或更多。爱丽丝吃了三颗糖果，总大小为 $1 + 4 + 1 = 6$
  ，糖果序列变为 $[5, 9, 2, 6, 5, 3]$。

- 第四步：爱丽丝在上一步吃了 $6$，这意味着鲍勃必须吃 $7$ 或更多。鲍勃吃了两颗糖果，总大小为 $3 + 5 = 8$
  ，糖果序列变为 $[5, 9, 2, 6]$。

- 第五步：鲍勃在上一步吃了 $8$，这意味着爱丽丝必须吃 $9$ 或更多。爱丽丝吃了两颗糖果，总大小为 $5 + 9 = 14$
  ，糖果序列变为 $[2, 6]$。

- 第六步（最后一步）：爱丽丝在上一步吃了 14，这意味着鲍勃必须吃 15 或更多。这是不可能的，所以鲍勃吃了剩下的两颗糖果，游戏结束。

输出游戏中的步数以及两个数字：

- a — 游戏中爱丽丝吃掉的所有糖果的总大小；

- b — 游戏中鲍勃吃掉的所有糖果的总大小。

### 代码

```C++
//
// Created by Luminous on 2024/10/2.
// https://codeforces.com/contest/1352/problem/D
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

deque<int> q;

void solve() {
    int n;
    cin >> n;
    for (int i = 0, x; i < n; i++) {
        cin >> x;
        q.push_back(x);
    }

    int time = 0, a = 0, b = 0, preSize = 0;
    while (!q.empty()) {
        time++;
        int size = 0;
        if ((time & 1) == 1) {
            while (!q.empty() && size <= preSize) {
                size += q.front();
                q.pop_front();
            }
            a += size;
        } else {
            while (!q.empty() && size <= preSize) {
                size += q.back();
                q.pop_back();
            }
            b += size;
        }
        preSize = size;
    }

    cout << time << " " << a << " " << b << endl;
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

## E. Special Elements

[原题地址](https://codeforces.com/contest/1352/problem/E)

### 题意

给定数组 $a = [a_{1}, ~a_{2}, ..., ~a_{n}]$ （$1 \leq a_{i} \leq n$）。如果存在一对索引 $l$ 和 $r$ （$1 \leq l \lt r \leq n$
），使得 $a_{l} = a_{l} + a_{l + 1} + ... + a_{r}$，则其元素 $a_{i}$
被称为特殊元素。换句话说，如果一个元素可以表示为数组中两个或更多连续元素的总和（无论它们是否为特殊元素），则该元素被称为特殊元素。

打印给定数组 $a$ 中特殊元素的数量。

例如，如果 $n = 9$ 且 $a = [3, 1, 4, 1, 5, 9, 2, 6, 5]$，则答案是 $5$：

- $a_{3} = 4$ 是一个特殊元素，因为 $a_{3} = 4 = a_{1} + a_{2} = 3 + 1$；

- $a_{5} = 5$ 是一个特殊元素，因为 $a_{5} = 5 = a_{2} + a_{3} = 1 + 4$；

- $a_{6} = 9$ 是一个特殊元素，因为 $a_{6} = 9 = a_{1} + a_{2} + a_{3} + a_{4} = 3 + 1 + 4 + 1$；

- $a_{8} = 6$ 是一个特殊元素，因为 $a_{8} = 6 = a_{2} + a_{3} + a_{4} = 1 + 4 + 1$；

- $a_{9} = 5$ 是一个特殊元素，因为 $a_{9} = 5 = a_{2} + a_{3} = 1 + 4$。

请注意，数组 $a$ 的某些元素可能相等 - 如果几个相等的元素是特殊的，那么它们都应在答案中计算。

### 代码

```C++
//
// Created by Luminous on 2024/10/2.
// https://codeforces.com/contest/1352/problem/E
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 8e3 + 10;

int a[MAX_N];
bool book[MAX_N];

void init(int n) {
    fill(book, book + n + 10, false);
}

void solve() {
    int n;
    cin >> n;
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }

    init(n);
    for (int i = 0; i < n; i++) {
        int sum = a[i];
        for (int j = i + 1; j < n; j++) {
            sum += a[j];
            if (sum <= n) {
                book[sum] = true;
            }
        }
    }

    int ans = 0;
    for (int i = 0; i < n; i++) {
        if (book[a[i]]) {
            ans++;
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

## F. Binary String Reconstruction

[原题地址](https://codeforces.com/contest/1352/problem/F)

### 题目类型

构造

### 题意

对于某些二进制字符串 $s$（即每个字符 $s_i$ 要么是 `'0'` 要么是 `'1'`
），写下了所有连续（相邻）字符对。换句话说，写下了所有长度为 $2$ 的子串。对于每个对（长度为 $2$ 的子串），计算其中 `'1'`（$1$ 的数量）。

您将获得三个数字：

- $n_{0}$ — $1$ 的数量等于 $0$ 的连续字符对（子串）的数量；

- $n_{1}$ — $1$ 的数量等于 $1$ 的连续字符对（子串）的数量；

- $n_{2}$ — $1$ 的数量等于 $2$ 的连续字符对（子串）的数量。

例如，对于字符串 `s = "1110011110"`，将写下以下子串：`"11"`、`"11"`、`"10"`、`"00"`、`"01"`、`"11"`、`"11"`、`"11"`、`"10"`
。因此，$n_{0}=1$，$n_{1}=3$，$n_{2}=5$。

您的任务是根据给定的 $n_{0}$、$n_{1}$、$n_{2}$ 值恢复任何合适的二进制字符串 `s`。保证至少 $n_{0}$、$n_{1}$、$n_{2}$
中的一个数字大于 $0$。并且保证存在解决方案。

### 代码

```C++
//
// Created by Luminous on 2024/9/30.
// https://codeforces.com/contest/1352/problem/F
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    int n0, n1, n2;
    cin >> n0 >> n1 >> n2;

    if (n2 != 0) {
        for (int i = 0; i < n2 + 1; i++) {
            cout << 1;
        }
    }

    if (n0 != 0) {
        for (int i = 0; i < n0 + 1; i++) {
            cout << 0;
        }
    }

    if (n1 > 0) {
        if (n2 == 0 && n0 != 0) {
            for (int i = 0, x = 1; i < n1; i++, x ^= 1) {
                cout << x;
            }
        } else if (n2 != 0 && n0 == 0) {
            for (int i = 0, x = 0; i < n1; i++, x ^= 1) {
                cout << x;
            }
        } else if (n2 != 0 && n0 != 0) {
            for (int i = 0, x = 1; i < n1 - 1; i++, x ^= 1) {
                cout << x;
            }
        } else if (n2 == 0 && n0 == 0) {
            for (int i = 0, x = 1; i < n1 + 1; i++, x ^= 1) {
                cout << x;
            }
        }
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

## G. Special Permutation

[原题地址](https://codeforces.com/contest/1352/problem/G)

### 题目类型

构造

### 题意

长度为 $n$ 的排列是一个数组 $p=[p_{1}, ~p_{2}, ~\cdots,~p_{n}]$ ，其中包含从 $1$ 到 $n$（包括 $1$ 和 $n$
）的每个整数，并且每个数字恰好出现一次。例如，$p=[3,1,4,2,5]$ 是长度为 $5$ 的排列。

对于给定的数字 $n$（$n \geq 2$），找到一个排列 $p$，其中任意两个相邻（相邻）元素的绝对差（即差的绝对值）在 $2$ 到 $4$
之间（包括 $2$ 和 $4$）。形式上，找到这样的排列 $p$，对于每个 $i$（$1 \leq i \leq n$
），都有 $2 \leq \lvert pi−pi+1 \rvert \leq 4$。

为给定的整数 $n$ 打印任何这样的排列，或者确定它不存在。

### 代码

```C++
//
// Created by Luminous on 2024/10/2.
// https://codeforces.com/contest/1352/problem/G
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    int n;
    cin >> n;

    if (n <= 3) {
        cout << -1 << endl;
        return;
    }

    if (n == 4) {
        cout << "3 1 4 2" << endl;
        return;
    }

    for (int i = 1; i <= n; i += 2) {
        cout << i << " ";
    }

    if (n % 2 == 1) {
        n--;
        cout << n - 2 << " " << n << " ";
        for (int i = n - 4; i > 0; i -= 2) {
            cout << i << " ";
        }
        cout << endl;
    } else {
        cout << n - 4 << " " << n << " " << n - 2 << " ";
        for (int i = n - 6; i > 0; i -= 2) {
            cout << i << " ";
        }
        cout << endl;
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