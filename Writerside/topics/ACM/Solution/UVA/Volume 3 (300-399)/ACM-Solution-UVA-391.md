# UVA 391 - Mark-up

## 🚀 原题地址
[UVA 391 - Mark-up](https://onlinejudge.org/external/3/391.pdf)

## 🏷️ 题目类型

模拟

## 📜 题目

**题意**

标记语言是有助于格式化文本文件的计算机语言。使用特殊关键字来标记文本，以控制字体、页面样式、段落样式等。`TEX`、`troff` 和 `HTML` 是
标记语言的示例。拼写检查可能难以适应这些特殊文本。通常，必须创建特殊的处理器或拼写检查器以适应标记语言。特殊处理器将识别标记语言并将其从文本中剥离，
以便“纯”文本随后可由拼写检查器处理。对于此问题，您要为一种小型标记语言编写这样的处理器，以便您的程序的输出将是没有标记的纯文本。要考虑的标记语言是
一种允许在文本内修改字体的语言。每个标记命令前面都有一个 `\` 字符。如果 `\` 字符后面的字母不是下面表格中识别的命令，则 `\` 后面的字符将作为纯文本
的一部分打印。例如，标记 `\\` 可用于打印单个 `\` 。

- `\b` 切换粗体字体的开/关（默认状态为关）

- `\i` 切换斜体字体的开/关（默认状态为关）

- `\s` 设置字体大小；`s` 后面紧跟一个可选数字；如果数字缺失，则该命令将恢复先前的大小

- `\*` 切换标记的处理开/关；如果处理被关闭，则标记被视为文字文本（默认状态为开）

`\s` 命令后面的数字可以有小数点，因此 $12$、$9.5$、$11$. 和 $.5$ 都应被识别为有效数字。


**输入 & 输出**

输入文件将是包含上述语言标记的纯文本。一开始，应开启标记的处理。应处理该文件，直至遇到文件结束。

**样例输入**

```text
\s18.\bMARKUP sample\b\s
\*For bold statements use the \b command.\*
If you wish to \iemphasize\i something use the \\i command.
For titles use \s14BIG\s font sizes, 14 points usually works well.
Remember that all of the commands toggle except for the \\s command.
```

**样例输出**

```text
MARKUP sample
For bold statements use the \b command.
If you wish to emphasize something use the \i command.
For titles use BIG font sizes, 14 points usually works well.
Remember that all of the commands toggle except for the \s command.
```

## 🔍 分析

共有 $5$ 种标签需要处理，其中 `\i`、 `\b`、`\s` 标签只需要把属于标签的部分去除（`\s` 包括其后面的数字和 `.`）。`\\` 需要输出一个 `\`。
而 `\*` 相对麻烦一点，需要考虑标签闭合和不闭合两种情况，不闭合则将后面所有内容原样输出即可，如果闭合则需要将两个 `\*` 标签之间的内容原样输出。
可以维护一个变量来标记 `\*` 左标签是否开启。

## 💡 代码

```C++
//
// Created by Luminous on 2024/10/23.
// https://onlinejudge.org/external/3/391.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

void solve() {
    bool flag = false;
    string s;
    while (getline(cin, s)) {
        s.push_back('\n');
        for (int i = 0; i < s.length(); i++) {
            if (!flag) {
                if (s[i] == '\\') {
                    if (s[i + 1] == '\\') {
                        cout << '\\';
                        i++;
                    } else if (s[i + 1] == 'i' || s[i + 1] == 'b') {
                        i++;
                    } else if (s[i + 1] == 's') {
                        int j = i + 1;
                        while (j + 1 < s.length() && (s[j + 1] == '.' || isdigit(s[j + 1]))) {
                            j++;
                        }
                        i = j;
                    } else if (s[i + 1] == '*') {
                        flag = true;
                        i++;
                    }
                } else {
                    cout << s[i];
                }
            } else {
                if (s[i] == '\\' && s[i + 1] == '*') {
                    flag = false;
                    i++;
                } else {
                    cout << s[i];
                }
            }
        }
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