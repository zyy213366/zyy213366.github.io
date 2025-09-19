---
title: 二分三分以及01分数规划
date: 2025-07-25 12:44:22
tags: acm培训
cover: /img/02.jpg
---
## 二分三分以及01分数规划
### 二分算法
二分算法的本质是求严格增或严格减函数，数列，数组中的某一特值，通过将整个区间无限对半开，找到答案所在的区间，经过多次迭代后区间左边界等于区间右边界，或两者无限逼近，那么这个答案就是解。
既然知道二分就是对于n个数对半开，那么如果将查询每一个数的时间复杂度视作为单位时间复杂度o（1），二分的时间复杂度就为o（log n），这比在数组中遍历每个元素o（n），快上很多。
在一般算法竞赛中或者实际生活中，有时会需要我们去用到二分算法，那么他的标准模板是什么呢？我们如何在c++中运用它呢？
```cpp
int binarySearch(vector<int>& a, int target) {
    int l = 0, r = a.size() - 1;
    while (l <= r) {
        int mid = (l + r) / 2;
        if (a[mid] == target) return mid;
        else if (a[mid] < target) l = mid + 1;
        else r = mid - 1;
    }
    return -1;  // 没找到
}
```
首先在二分函数中确定左边界与右边界，通过一个while循环来实现递归的目的，在while中建立一个mid变量，来让他作为新的左右边界来求出答案。
手写二分算法往往是很麻烦的事情，不过在c++中，为了求某些特殊情况，c++会给你一些封装的stl来实现。比如你要求数组中最小的元素或者最大的元素，你可以通过lower_bound()或upper_bound()方法来查询，这并不要求数组有序，因为排序的操作是在你使用时候自动进行了，它的时间复杂度也是o（log n）。
### 三分算法
三分算法顾名思义就是把一个数组或区间分为三份，通常用于求凹凸函数的最大值最小值。本质类似与二分，先给出左右端点，再求出两个三等分点，将两个三等分点的函数值进行比较，判断答案处于哪个区间内，然后把其中一个三等分点作为新的端点，进行迭代，求出答案。
```cpp
int ternarySearch(int l, int r) {
    while (r - l >= 3) {
        int mid1 = l + (r - l) / 3;
        int mid2 = r - (r - l) / 3;
        if (f(mid1) < f(mid2))
            r = mid2 - 1;  // 最优解在 [l, mid2-1]
        else
            l = mid1 + 1;  // 最优解在 [mid1+1, r]
    }

    int res = l;
    for (int i = l + 1; i <= r; ++i)  // 最后暴力找出最小
        if (f(i) < f(res)) res = i;
    return res;
}
```
### 01分数规划
题面详见 https://ac.nowcoder.com/acm/contest/22353/1011
这道题是01背包问题的变种，可以用二分去写，思路就是先用结构体定义每个物品，将所求 ∑w/∑v=r，r要max转化为 ∑w-r*∑v=0，找到最大的r。
```cpp
#include <bits/stdc++.h>
using namespace std;
int n,k;
struct item{
    double w,v;
};
vector <item> items;
bool check(double R){
    vector <double> temps;
    for(int i=0;i<items.size();++i){
        temps.push_back(items[i].v-R*items[i].w);
    }
    sort(temps.rbegin(),temps.rend());
    double sum=0;
    for(int i=0;i<k;++i){
        sum+=temps[i];
    }
    return sum>=0;
}
int main(){
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cin>>n>>k;
    items.resize(n);
    for(int i=0;i<n;++i){
        cin>>items[i].w>>items[i].v;
    }
    double left=0;double right=1000001;
    for(double i=0;i<100;++i){
        double mid=left+(right-left)/2;
        if(check(mid)){
            left=mid;
        }else{
            right=mid;
        }
    }
    cout<<fixed<<setprecision(10)<<left<<endl;
    return 0;
}
```
