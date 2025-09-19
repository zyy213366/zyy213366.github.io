---
title: 欧几里得的游戏
date: 2025-08-04 16:11:04
tags: acm培训
cover: /img/02.jpg
---
本来我是完全没有接触过博弈论的，但是今天看到了这道题，用到了Euclidean Game 定理。搜了一下，发现这玩意好像有点意思，就写一下。
### 题干
欧几里德的两个后代 Stan 和 Ollie 正在玩一种数字游戏，这个游戏是他们的祖先欧几里德发明的。给定两个正整数 M 和 N，从 Stan 开始，从其中较大的一个数，减去较小的数的正整数倍，当然，得到的数不能小于 0 。然后是 Ollie，对刚才得到的数，和 M , N 中较小的那个数，再进行同样的操作……直到一个人得到了0，他就取得了胜利。下面是他们用(25,7) 两个数游戏的过程：

初始：(25,7)；
Stan：(11,7)；
Ollie：(4,7)；
Stan：(4,3)；
Ollie：(1,3)；
Stan：(1,0)。
Stan 赢得了游戏的胜利。

现在，假设他们完美地操作，谁会取得胜利呢？

Input
本题有多组测试数据。
第一行为测试数据的组数 C 。 下面 C 行，每行为一组数据，包含两个正整数 M , N(M , N<2^31)M,N(M,N<2^31)。

Output
对每组输入数据输出一行，如果 Stan 胜利，则输出 Stan wins；否则输出 Ollie wins。

```cpp
#include <bits/stdc++.h>
using namespace std;

bool solve(int a, int b) {
    if (b == 0) return false;
    if (a / b > 1) return true;
    return !solve(b, a % b);  // 当前取一步，能不能让对方输
}

int main() {
    int T;
    cin >> T;
    while (T--) {
        int a, b;
        cin >> a >> b;
        if (a < b) swap(a, b);
        if (solve(a, b)) cout << "Stan wins" << endl;
        else cout << "Ollie wins" << endl;
    }
    return 0;
}

```
### 证明过程
本道题就是分成两部分，当k只能取一个值的时候取决于之后的状态，当有多个状态时候直接必胜，那么必胜的证明过程如下。
![证明过程](/img/ojilide.jpg)