# UVA 123 - Searching Quickly

## 原题地址

[UVA 123 - Searching Quickly](https://onlinejudge.org/external/1/123.pdf)

## 🏷️ 题目类型

模拟、排序


## 📜 题目


**题意**

搜索和排序是计算机科学的理论和实践的一部分。例如，二分查找提供了一个易于理解的具有次线性复杂度的算法的好例子。快速排序是一种高效的 $O(n \log{n})$
（平均情况）基于比较的排序。

`KWIC` 索引是一种索引方法，允许对例如标题列表进行高效的“人工搜索”。

给定一个标题列表和一个“要忽略的单词”列表，您要编写一个程序来生成标题的`KWIC`（上下文关键词）索引。在 `KWIC` 索引中，对于标题中出现的每个关键字，
标题都会列出一次。`KWIC` 索引按关键字进行字母排序。 任何不是“要忽略的单词”之一的单词都是潜在的关键字。

例如，如果要忽略的单词是“`the`，`of`，`and`，`as`，`a`”，标题列表是：
<br/>
`Descent of Man`
<br/>
`The Ascent of Man`
<br/>
`The Old Man and The Sea`
<br/>
`A Portrait of The Artist As a Young Man`
<br/>

这些标题的 KWIC 索引可能是：
<br/>
`a portrait of the ARTIST as a young man`
<br/>
`the ASCENT of man`
<br/>
`DESCENT of man`
<br/>
`descent of MAN`
<br/>
`the ascent of MAN`
<br/>
`the old MAN and the sea`
<br/>
`a portrait of the artist as a young MAN`
<br/>
`the OLD man and the sea`
<br/>
`a PORTRAIT of the artist as a young man`
<br/>
`the old man and the SEA`
<br/>
`a portrait of the artist as a YOUNG man`

**输入**

输入是一系列的行，字符串`"::"`
用于将要忽略的单词列表与标题列表分开。每个要忽略的单词本身以小写字母出现在一行中，长度不超过 $10$
个字符。每个标题本身出现在一行中，可能由大小写（大写和小写）字母组成。标题中的单词由空格分隔。没有标题包含超过 $15$
个单词。要忽略的单词不超过 $50$ 个，标题不超过 $200$ 个，标题和要忽略的单词组合不超过 $10,000$ 个字符。输入中除了`'a'`–
`'z'`、`'A'–'Z'`和空格之外不会出现其他字符。


**输出**

输出应该是标题的 `KWIC` 索引，每个标题针对标题中的每个关键字出现一次，并且 `KWIC`
索引按关键字字母顺序排列。如果一个单词在标题中出现多次，每个实例都是一个潜在的关键字。

关键字应全部以大写字母出现。标题中的所有其他单词应小写。**具有相同关键字的 `KWIC` 索引中的标题应按照在输入文件中出现的相同顺序出现**。**如果在同一标题中一个单词的多个实例是关键字，则关键字应按从左到右的顺序大写**。在确定一个单词是否应被忽略时，大小写（大写或小写）无关紧要。
`KWIC` 索引中的标题无需根据关键字进行对齐或调整，所有标题都可以左对齐列出。

**样例输入**

```text
is
the
of
and
as
a
but
::
Descent of Man
The Ascent of Man
The Old Man and The Sea
A Portrait of The Artist As a Young Man
A Man is a Man but Bubblesort IS A DOG
```

**样例输出**

```text
a portrait of the ARTIST as a young man
the ASCENT of man
a man is a man but BUBBLESORT is a dog
DESCENT of man
a man is a man but bubblesort is a DOG
descent of MAN
the ascent of MAN
the old MAN and the sea
a portrait of the artist as a young MAN
a MAN is a man but bubblesort is a dog
a man is a MAN but bubblesort is a dog
the OLD man and the sea
a PORTRAIT of the artist as a young man
the old man and the SEA
a portrait of the artist as a YOUNG man
```


## 🔍 分析

更具题目可以知道对一个标题排序以及输出需要以下几个信息：

- 关键字
- 在文中出现的顺序
- 标题变为关键字大写，其它单词小写的形式

用一个结构体封装这些信息，并将所有的结构体求出来（每个标题中除了忽略词外，每个单词都做一次关键词）。然后对这些结构体进行排序：

- 先按关键字排序
- 关键字相同的按在文中出现顺序排序
- 出现顺序相同的按字典序排序（即一个标题中若存在多个相同关键字，则关键从左到又大写）


## 💡 代码

```C++
//
// Created by Luminous on 2024/10/16.
// https://onlinejudge.org/external/1/123.pdf
//

#pragma GCC optimize(3)

#include <bits/stdc++.h>
using namespace std;

#define endl "\n"


const int MAX_N = 2e5 + 10;

struct Title {
    string keyWord;
    string title;
    int index;

    bool operator<(const Title &rhs) const {
        if (keyWord < rhs.keyWord) return true;
        if (keyWord > rhs.keyWord) return false;
        if (index < rhs.index) return true;
        if (index > rhs.index) return false;
        return title < rhs.title;
    }
};

unordered_map<string, bool> ignoreWords;
vector<Title> titles;


string toLower(string word, int start, int end) {
    transform(word.begin() + start, word.begin() + end, word.begin() + start, ::tolower);
    return word;
}

string toUpper(string word, int start, int end) {
    transform(word.begin() + start, word.begin() + end, word.begin()  + start, ::toupper);
    return word;
}


void solve() {
    string ignoreWord;
    while (cin >> ignoreWord && ignoreWord != "::") {
        ignoreWord = toUpper(ignoreWord, 0, ignoreWord.size());
        ignoreWords[ignoreWord] = true;
    }

    cin.ignore();

    int index = 0;
    string title;
    while (getline(cin, title)) {
        if (title == "-1") break;

        title = toLower(title, 0, title.size());
        for (int i = 0; i < title.size(); i++) {
            if (title[i] == ' ') continue;

            int j = i;
            while (j < title.size() && title[j] != ' ') j++;

            string word = title.substr(i, j - i);
            word = toUpper(word, 0, word.size());
            if (!ignoreWords[word]) {
                titles.push_back({
                    word,
                    toUpper(title, i, j),
                    index++,
                });
            }

            i = j;
        }
    }

    sort(titles.begin(), titles.end());

    for (auto &item : titles) {
        cout << item.title << endl;
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

