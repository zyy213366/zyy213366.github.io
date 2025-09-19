---
title: 并查集，优先队列，双端队列
date: 2025-08-07 16:07:06
tags: acm培训
cover: /img/02.jpg
---
### 并查集
当我们碰到堆的划分问题时候我们可以用到并查集，其实就是每个节点通过一个父节点来存储信息，然后通过递归的方法将一个堆中所有子节点或叶子节点的父节点全部指向根节点，而根节点的父节点指向自己。
本来我没有了解到这一层的时候认为堆的合并将会是很困难的事，因为每个父节点是不一样的，但现在就好办了，我们只要把两个根节点给合并，最后再递归一下就算成功了。
它的时间复杂度比o（log n）很快，是反阿克曼函数，接近于o（1）。
```cpp
int find_set(int i) {
    if (fa[i] == i) return i;
    return fa[i] = find_set(fa[i]);
}

void union_set(int a, int b) {
    a = find_set(a);
    b = find_set(b);
    if (a != b) fa[b] = a;
}
```
这是并查集的设置和合并，有趣的一个点是合并不会立刻发生，原来第二堆的值仍然指向第二个根节点，而第二个根节点指向第一个，真正完全的合并在之后再次查询并查集时候find_set上自动操作的。

### 双端队列
双端队列两边都能进出元素，就是队列和栈的集合，直接来用法。
```cpp
int main() {
    Deque dq;

    dq.push_back(10);
    dq.push_front(5);
    dq.push_back(20);

    cout << "Front: " << dq.front() << endl;  // 5
    cout << "Back: " << dq.back() << endl;    // 20

    dq.pop_front();
    cout << "New Front: " << dq.front() << endl;  // 10

    dq.pop_back();
    cout << "New Back: " << dq.back() << endl;    // 10

    cout << "Size: " << dq.size() << endl;        // 1

    return 0;
}

```

### 优先队列
优先队列本质是通过红黑树来实现的，默认是从大到小排序的队列，实际也不能说是排序，其中只有最大值是有序的，我拿出最大值之后又是新的最大值有序（在顶层），这样看上去像是排完了序，这种被称为堆排序。时间复杂度o(log n)。
我们来看用法。
```cpp
#include <iostream>
#include <queue>
#include <vector>
#include <functional> // std::greater

using namespace std;

// 自定义结构体
struct Node {
    int val, priority;
    Node(int v, int p) : val(v), priority(p) {}
};

// 自定义比较函数，用于优先级队列中从小到大排列
struct Compare {
    bool operator()(const Node& a, const Node& b) {
        return a.priority > b.priority; // 小优先
    }
};

int main() {
    // 1. 默认大根堆（从大到小）
    priority_queue<int> maxHeap;
    maxHeap.push(5);
    maxHeap.push(1);
    maxHeap.push(10);

    cout << "大根堆: ";
    while (!maxHeap.empty()) {
        cout << maxHeap.top() << " ";
        maxHeap.pop();
    }
    cout << endl;

    // 2. 小根堆（从小到大）
    priority_queue<int, vector<int>, greater<int>> minHeap;
    minHeap.push(5);
    minHeap.push(1);
    minHeap.push(10);

    cout << "小根堆: ";
    while (!minHeap.empty()) {
        cout << minHeap.top() << " ";
        minHeap.pop();
    }
    cout << endl;

    // 3. 自定义结构体优先队列（根据 priority 从小到大）
    priority_queue<Node, vector<Node>, Compare> customHeap;
    customHeap.push(Node(100, 5));
    customHeap.push(Node(200, 1));
    customHeap.push(Node(300, 10));

    cout << "结构体小根堆 (按 priority): ";
    while (!customHeap.empty()) {
        Node node = customHeap.top();
        cout << "(" << node.val << ", priority=" << node.priority << ") ";
        customHeap.pop();
    }
    cout << endl;

    return 0;
}

```
over

