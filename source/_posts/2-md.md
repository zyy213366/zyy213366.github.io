---
title: c++中dfs以及bfs代码分享
date: 2024-12-15 15:13:44
tags: 蓝桥杯培训
cover: /img/02.jpg
---
## c++中dfs以及bfs代码分享
### 深度优先搜索（DFS，Depth-First Search）
深度优先搜索（DFS） 是一种遍历或搜索树或图的算法。它的基本思想是从根节点（或任意一个起始节点）开始，沿着一个分支一直走到底（即沿着每条边走到尽头），然后回溯，继续沿其他分支进行搜索，直到遍历完所有节点。

DFS 的核心特点是“深度优先”，即每次深入到树的子节点，直到没有子节点为止，然后回退到父节点继续搜索。

**基本过程：**
1. 从起始节点开始，首先访问该节点。
2. 访问该节点的一个未被访问的邻居节点，继续深度遍历。
3. 直到所有邻居节点都被访问过后，回到上一个节点，继续访问其未被访问的邻居。
4. 重复上述过程直到遍历完整个图或树。
```基本的DFS伪代码：sql

DFS(Graph, start):
    1. Mark start node as visited
    2. For each neighbor of start:
    3. If the neighbor is not visited, recursively call DFS on it
```
**图的表示：**
图可以使用 邻接矩阵 或 邻接表 来表示。DFS 都可以在这两种结构上实现。
DFS 的应用：
**路径查找：** DFS 可以用于图的路径查找，尤其是在查找所有路径或最深路径时很有效。
**拓扑排序：** 在有向无环图（DAG）中，DFS 是实现拓扑排序的关键算法。
**寻找连通分量：** 可以用 DFS 来查找图中所有的连通分量。
**迷宫问题：** DFS 常用于解决迷宫问题，寻找从起点到终点的路径。
图的强连通分量：例如，在求解强连通分量时，可以用 DFS 配合 Tarjan 算法来实现。
**DFS 算法的时间复杂度：**
如果使用邻接表表示图，DFS 的时间复杂度是 O(V + E)，其中 V 是节点数，E 是边数。
如果使用邻接矩阵表示图，DFS 的时间复杂度是 O(V^2)。
示例：用 C++ 实现 DFS
以下是一个用 C++ 实现的简单图的 DFS 算法：

```cpp

#include <iostream>
#include <vector>
using namespace std;

void dfs(int node, vector<vector<int>>& graph, vector<bool>& visited) {
    // 打印当前节点并标记为已访问
    cout << node << " ";
    visited[node] = true;

    // 递归访问所有未访问的邻居节点
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor, graph, visited);
        }
    }
}

int main() {
    int n, e;  // n:节点数, e:边数
    cin >> n >> e;

    // 图的邻接表表示
    vector<vector<int>> graph(n);

    // 读入图的边
    for (int i = 0; i < e; ++i) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);
        graph[v].push_back(u);  // 无向图
    }

    // 访问标记数组
    vector<bool> visited(n, false);

    // 从节点 0 开始 DFS
    cout << "DFS Traversal: ";
    dfs(0, graph, visited);

    return 0;
}
```

1. **图的表示：**
   使用邻接表 ```graph``` 来存储图的结构。```graph[u]``` 存储与节点 u 相邻的所有节点。
2. **DFS 函数：**
```
dfs(int node, vector<vector<int>>& graph, vector<bool>& visited)：
```
先标记当前节点为已访问。
然后递归地访问当前节点的所有未访问过的邻居节点。
3. **主函数：**

读入图的节点数 n 和边数 e，然后通过输入构造图的邻接表。
最后调用 DFS 函数从节点 0 开始遍历图。


### BFS（广度优先搜索）算法介绍
广度优先搜索（BFS，Breadth-First Search） 是一种用于遍历或搜索图（Graph）或树（Tree）中节点的算法。其基本思想是从一个起始节点开始，沿着图的边逐层访问邻接节点，直到遍历完所有的节点。

**BFS 的核心思想：**
BFS 是一种 层次遍历 的算法，首先访问起始节点，再访问与之相邻的节点，然后是这些相邻节点的相邻节点，依此类推，逐层访问节点，直到图中所有可达的节点都被访问。

**BFS 的重要特点是：**

逐层访问：它会先访问当前节点的所有邻居，然后再访问它们的邻居，层次逐步加深。
最短路径：在无权图中，BFS 可以找到从起点到目标节点的 最短路径。
**BFS 的基本过程：**
1. **初始化：** 从起始节点开始，加入一个 队列（queue），并标记为已访问。创建一个访问标记数组，用来记录哪些节点已经被访问过，防止重复访问。
2. **遍历：** 从队列中取出一个节点，访问它，并将所有未被访问过的邻居节点加入队列。对队列中的节点重复上述操作，直到队列为空，说明所有可以到达的节点都已被访问。
```BFS 算法步骤（伪代码）：

BFS(Graph, start):
    1. Create a queue Q
    2. Mark start as visited and enqueue start into Q
    3. While Q is not empty:
        a. Dequeue a node from Q
        b. For each neighbor of the node:
            i. If neighbor is not visited:
                - Mark neighbor as visited
                - Enqueue neighbor into Q
```
```BFS 的图示例：
假设我们有以下图结构：

      A
     / \
    B   C
   / \
  D   E
```
**BFS 从节点 A 开始遍历：**

1. 从 A 开始，访问 A。
2. 访问 A 的邻居 B 和 C，将它们加入队列。
3. 取出 B，访问 B，访问 B 的邻居 D 和 E，将它们加入队列。
4. 取出 C，访问 C，但 C 没有新的邻居。
5. 取出 D，D 没有新的邻居。
6. 取出 E，E 没有新的邻居。
<div style="text-align: left;">
  BFS 的遍历顺序为：A -> B -> C -> D -> E
</div>

**BFS 算法的实现**
在实现 BFS 时，我们使用 队列（Queue） 来存储待访问的节点。队列的先进先出（FIFO）特性保证了我们按层次顺序访问节点。

```C++ 实现 BFS
cpp

#include <iostream>
#include <vector>
#include <queue>
using namespace std;

void bfs(int start, vector<vector<int>>& graph) {
    vector<bool> visited(graph.size(), false);  // 访问标记
    queue<int> q;  // 创建队列

    visited[start] = true;  // 标记起始节点为已访问
    q.push(start);  // 将起始节点入队

    while (!q.empty()) {
        int node = q.front();  // 取队列头部元素
        q.pop();  // 弹出队列头部元素

        cout << node << " ";  // 访问当前节点

        // 遍历当前节点的所有邻居
        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;  // 标记为已访问
                q.push(neighbor);  // 将邻居节点入队
            }
        }
    }
}

int main() {
    int n, e;  // n: 节点数, e: 边数
    cin >> n >> e;

    vector<vector<int>> graph(n);  // 图的邻接表表示

    // 读入图的边
    for (int i = 0; i < e; ++i) {
        int u, v;
        cin >> u >> v;
        graph[u].push_back(v);  // 无向图
        graph[v].push_back(u);  // 无向图
    }

    // 从节点 0 开始 BFS
    cout << "BFS Traversal: ";
    bfs(0, graph);

    return 0;
}
```
**代码解析：**
**邻接表：** 图通过邻接表 graph 来表示，graph[u] 存储与节点 u 相邻的所有节点。
**队列：** 队列 q 用来存储待访问的节点，先进先出（FIFO）保证了按层次顺序访问节点。
**访问标记：** visited 数组记录哪些节点已经被访问过，防止重复访问。
BFS 遍历：从起始节点 start 开始，先访问它的邻居，再逐层访问其他节点。
