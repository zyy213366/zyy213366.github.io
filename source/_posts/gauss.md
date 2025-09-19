---
title: 高斯消元，快速幂和一些拓展
date: 2025-08-04 13:47:14
tags: acm培训
cover: /img/02.jpg
---
### 标准快速幂
平时计算幂次时计算机的一般方法其实就是累乘，比如2^8就是2*2*2*2一直乘下去，这带来了极高的时间复杂度，大概是o（n），那么快速幂就是对于这种计算方法的优化，继续拿2的8次方举例，他会判断幂次是奇数还是偶数，发现是偶数，先对底数进行平方，变成4^4，然后继续操作，时间复杂度大概是o（logn）。这为什么能加快速度，其实是因为这样计算减少了计算规模。当然使用这种方法必须让你手写一个函数。
```cpp
long long qpow(long long a, long long n, long long mod) {
    long long res = 1;
    while (n > 0) {
        if (n & 1) res = res * a % mod; // 当前位是1
        a = a * a % mod;  // a翻倍
        n >>= 1; // 指数右移，相当于除以2
    }
    return res;
}
```
实现快速幂首先需要定义一个res，它的作用是存储答案和遇到奇数次幂时乘以底数，比如2^9，res会自乘一个2，然后剩下就变成2的8次方，可以执行我之前说的操作了。
_________________
### 矩阵快速幂
矩阵快速幂就是标准快速幂的一个拓展，面对矩阵的幂次运算，我们也通过相同的方式，唯一的区别就是底数变成了矩阵，我们需要生成单位矩阵，对矩阵进行赋值等等，原本时间复杂度是o（n^3*k），优化后大概是o（n^3*log k），其中n是矩阵大小，k是计算次数。
```cpp
// 矩a阵结构体
struct Matrix {
int n, m; // n: 行数, m: 列数
vector<vector<ll>> a;
Matrix(int _n, int _m) : n(_n), m(_m), a(_n + 1, vector<ll>(_m + 1, 0)) {}

    static Matrix identity(int n) {
        Matrix I(n, n);
        for (int i = 1; i <= n; ++i) {
            I.a[i][i] = 1;
        }
        return I;
    }
};
// 矩阵乘法: C = A * B
// 时间复杂度: O(n^3) 或 O(A.n * A.m * B.m)
Matrix operator*(const Matrix& A, const Matrix& B) {
    Matrix C(A.n, B.m);
    for (int i = 1; i <= A.n; ++i) {
        for (int j = 1; j <= B.m; ++j) {
            for (int k = 1; k <= A.m; ++k) {
                C.a[i][j] = (C.a[i][j] + A.a[i][k] * B.a[k][j]) % MOD;
            }
        }
    }
    return C;
}

// 矩阵快速幂: A^p, 时间复杂度: O(n^3 * log p) , A必须是方阵
Matrix power(Matrix A, ll p) {
    Matrix res = Matrix::identity(A.n);
    while (p) {
        if (p & 1) res= res * A;
            A = A * A;
            p >>= 1;
        }
        return res;
    }
```
这些代码最后一部分和标准快速幂相同，就是前面多了定义乘法和结构体部分。
_________________
### 高斯消元
既然我们以及介绍了有关矩阵快速幂的一些知识，后面就可以介绍高斯消元了。线代里高斯消元是通过矩阵多次初等变换来实现的，交换行列位置什么的。总结一下流程，其实就是面对一个方阵，先计算第一列，我们需要找到其中不为0的一项把他那一行提到最高位，并且把那一行归一化处理，后面几行的第一列全部消为0，然后把第二列不为零那一项提到次高位，归一化处理，然后其他全部消为0，以此类推直到结束。那么我们如何用c++来实现这一过程呢？？
```cpp
int gauss_jordan(vector<vector<double>>& a, int vars) {
    int eqs = a.size();
    if (eqs == 0) return 0;
    int rk = 0;
    for (int col = 0; col < vars && rk < eqs; ++col) {
        int pivot_row = rk;
        for (int i = rk + 1; i < eqs; ++i) {
            if (abs(a[i][col]) > abs(a[pivot_row][col])) {
                pivot_row = i;
            }
        }
        swap(a[rk], a[pivot_row]);
        if (abs(a[rk][col]) < EPS) {
            continue; 
        }
        double pivot_val = a[rk][col];
        for (int j = col; j < a[0].size(); ++j) {
            a[rk][j] /= pivot_val;
        }
        for (int i = 0; i < eqs; ++i) {
            if (i != rk) {
                double factor = a[i][col];
                for (int j = col; j < a[0].size(); ++j) {
                    a[i][j] -= factor * a[rk][j];
                }
            }
        }
        rk++;
    }
    return rk;
}
```
这部分代码套的最大的一个for循环是为了做之前提到的一列列处理的操作，里面第一个for循环是为了找不为零的那一项，swap一下提到目前应该处理的位置，也就是pivot_row，之后的for循环是归一化处理，接着一个for循环是把其余的几行那几项全部变为0，类推几次就差不多了，不过值得注意的是返回的是矩阵的秩，也就是rk，我们根据rk的值与vars比较，来判断解的个数，vars就是矩阵的行数。
|  条件  |  意义  |
|--------|-------|
|rk == vars 且无矛盾|唯一解|
|rk < vars 且无矛盾|无穷多解（自由变量存在）|