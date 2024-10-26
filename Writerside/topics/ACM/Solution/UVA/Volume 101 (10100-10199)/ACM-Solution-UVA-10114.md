# UVA 10114 - Loansome Car Buyer

## 🚀 原题地址

[UVA 10114 - Loansome Car Buyer](https://onlinejudge.org/external/101/10114.pdf)

## 🏷️ 题目类型

模拟

## 📜 题目

**题意**

卡拉·范（Kara Van）和李·萨布尔（Lee Sabre）很孤单。几个月前，他们贷款买了一辆新车，但现在周六晚上他们被困在家里，没车也没钱。你看，
发生了一起车祸，车子报废了。他们的保险赔付了 $10,000$ 美元，即汽车的当前价值。唯一的问题是，他们欠银行 $15,000$ 美元，而银行希望立即还款，
因为不再有汽车作为抵押品。就在片刻之间，这对不幸的夫妇不仅失去了他们的汽车，还额外损失了 $5,000$ 美元现金。卡拉和李没有考虑到的是折旧，即汽车随
着使用年限增加而价值降低。每个月，购车者的贷款还款会减少汽车欠款的金额。然而，每个月，随着汽车变老，它也会贬值。你的任务是编写一个程序，计算出购车
者欠款低于汽车价值的第一个时间（以月为单位）。对于这个问题，折旧是以上个月价值的百分比来指定的。

**输入**

输入包含若干贷款的信息。每笔贷款包含一行，其中有贷款的持续月数、首付、贷款金额以及随后的折旧记录数量。所有值均为非负，贷款最长为 $100$ 个月，
汽车价值最高为 $75,000$ 美元。由于折旧不是恒定的，不同的折旧率在一系列折旧记录中指定。每个折旧记录包含一行，其中有月份数和折旧百分比，
该百分比大于 $0$ 且小于 $1$。这些按月份严格递增排列，从第 $0$ 个月开始。第 $0$ 个月是将汽车从车行开走后立即适用的折旧，并且在数据中始终存在。
所有其他百分比是相应月份结束时的折旧金额。数据中可能未列出所有月份。如果某个月未列出，则采用之前的折旧百分比。
输入的结束由负的贷款持续月数表示 - 其他三个值将存在但不确定。为简单起见，我们假设是 $0%$ 利息的贷款，因此汽车的初始价值将是贷款金额加上首付。
汽车的价值和欠款可能是小于 $1$ 美元的正数。不要将值四舍五入到整分（$7,347.635$ 美元不应四舍五入到 $7,347.64$ 美元）。考虑下面第一个例子，
借款 $15,000$ 美元，为期 $30$ 个月。当买家将车从车行开走时，他仍欠款 $15,000$ 美元，但汽车价值已下降 $10\%$ 至 $13,950$ 美元。$4$ 个月后，
买家已支付了 $4$ 次，每次 $500$ 美元，并且汽车在第 $1$ 和第 $2$ 个月进一步折旧 $3\%$，在第 $3$ 和第 $4$ 个月折旧 $0.2\%$。此时，
汽车价值为 $13,073.10528$ 美元，借款人仅欠款 $13,000$ 美元。

**输出**

对于每笔贷款，输出的是借款人欠款少于汽车价值之前的完整月数。请注意，在英语中，除了 $1$ （`1 month`）以外，所有数值都需要使用复数形式（`5 months`）。

**样例输入**

```text
30 500.0 15000.0 3
0 .10
1 .03
3 .002
12 500.0 9999.99 2
0 .05
2 .1
60 2400.0 30000.0 3
0 .2
1 .05
12 .025
-99 0 17000 1
```

**样例输出**

```text
4 months
1 month
49 months
```

## 🔍 分析

根据题意模拟还款和汽车价值折损的过程即可。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/26.
// https://onlinejudge.org/external/101/10114.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

bool solve() {
    int n, m;
    double downPayment, loanAmount;
    cin >> n >> downPayment >> loanAmount >> m;

    if (n < 0) return false;

    double rates[n + 1];
    for (int i = 0; i < m; i++) {
        int k;
        double rate;
        cin >> k >> rate;

        for (int j = k; j <= n; j++) {
            rates[j] = rate;
        }
    }

    int mouth = 0;
    double carAmount = downPayment + loanAmount;
    double repaymentAmount = loanAmount / n;
    for (mouth = 0; mouth < n; mouth++) {
        carAmount = carAmount - carAmount * rates[mouth];
        if (loanAmount < carAmount) break;
        loanAmount -= repaymentAmount;
    }

    cout << mouth << " " << (mouth == 1 ? "month" : "months") << endl;

    return true;
}

int main() {

    ios::sync_with_stdio(false);

    while (solve());

    return 0;
}
```