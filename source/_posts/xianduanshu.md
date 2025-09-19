---
title: 线段树
date: 2025-07-25 15:05:11
tags: acm培训
cover: /img/02.jpg
---
线段树的设计是一颗二叉树，用来维护一个序列中各个区间的信息，像什么最大值，最小值，最大公因数等等，因为设计是二叉树，所以时间复杂度大约是（o logn），线段树的精髓是懒标记，懒标记是存储操作的方法，比如我们要在单点修改一个节点，不过操作不会马上进行，会存储在懒标记当中，然后在下次查询的时候执行操作，就可以压缩时间。
以下是基本的模板。
首先是建立线段树，通过递归建立左树和右数无限二分来实现的，先对叶子节点进行单独处理，然后就是对一般情况递归建立左右树直到遍历到叶子节点。
```cpp 
void build(int i, int l, int r) {
    tree[i].l = l;
    tree[i].r = r;
    if (l == r) {
        tree[i].val = arr[l]; // 叶子节点
        return;
    }
    int mid = (l + r) / 2;
    build(i*2, l, mid); //关键递归
    build(i*2+1, mid+1, r); //关键递归
    tree[i].val = tree[i*2].val + tree[i*2+1].val; // 维护区间和
}

```
这是针对于单点的更新操作，就是修改叶子节点往上推的过程，首先特判叶子节点，让其等于要修改的值，然后通过左右递归找到叶子节点，执行特判，然后通过叶子节点往上反推，计算出每个结点的值。
```cpp
void update(int i, int pos, int val) {
    if (tree[i].l == tree[i].r) {
        tree[i].val = val;
        return;
    }
    int mid = (tree[i].l + tree[i].r) / 2;
    if (pos <= mid) update(i*2, pos, val);
    else update(i*2+1, pos, val);
    tree[i].val = tree[i*2].val + tree[i*2+1].val;
}
```
这是查询某段区间的值，首先通过类似剪枝的操作，以防遍历一整棵树导致过大的时间复杂度，就是如果树上某个区间小于要查询的区间，就直接返回这段的值。否则就一直往下遍历到条件成立。
```cpp
int query(int i, int l, int r) {
    if (tree[i].l >= l && tree[i].r <= r) return tree[i].val;
    int mid = (tree[i].l + tree[i].r) / 2;
    int sum = 0;
    if (l <= mid) sum += query(i*2, l, r);
    if (r > mid) sum += query(i*2+1, l, r);
    return sum;
}

```