---
title: 25年蓝桥杯红黑树
date: 2025-04-12 19:27:58
tags: 蓝桥杯培训
cover: /img/02.jpg
---
以前听别人忽悠蓝桥杯会dfs暴力就能拿省三，我就赛前突击了一下dfs，没想到25蓝桥杯正好出了红黑树，就记录一下叭~~
## 题目
小蓝最近学习了红黑树，红黑树是一种特殊的二叉树，树上的结点有两种类型：红色结点和黑色结点。

小蓝在脑海中构造出一棵红黑树，构造方式如下：

根结点是一个红色结点；
如果当前结点 curNode 是红色结点，那么左子结点 curNode.left 是红色结点，右子结点 curNode.right 是黑色结点；
如果当前结点 curNode 是黑色结点，那么左子结点 curNode.left 是黑色结点，右子结点 curNode.right 是红色结点；
此二叉树前几层的形态如下图所示：

小蓝会从树上随机挑选结点，请你帮忙判断他选出的是红色结点还是黑色结点。

### 输入格式
输入的第一行包含一个正整数 m，表示小蓝挑选的结点数。 接下来 m 行，每行包含两个正整数 ni ,ki ，用一个空格分隔，表示小蓝挑选的结点是第 ni （从上往下数）第 ki个（从左往右数）结点。

### 输出格式
输出 m 行，每行包含一个字符串，依次表示小蓝每次挑选的结点的答案。RED 表示红色结点，BLACK 表示黑色结点。

```c++
#include <bits/stdc++.h>
using namespace std;
int dfs(int a,long long b){
    if(a==1 && b==1){ //根结点是一个红色结点
        return 1;
    }else{
        if(dfs(a-1,b%2+b/2)==1){ //如果当前结点 curNode 是红色结点，
            if(a%2==1){ //那么左子结点 curNode.left 是红色结点
                return 1;
            }else{ //右子结点 curNode.right 是黑色结点；
                return 0;
            }    
        }else{
            if(a%2==1){
                return 0;
            }else{
                return 1;
            }      
        }
    }
}
int main(){
    int m,a;
    long long b; //根据输入数据得出结论，超过2e5用long long
    cin>>m;
    for(int i=0;i<m;i++){
        cin>>a>>b;
        if(dfs(a,b)==1){
            cout<<"RED"<<endl;
        }else{
            cout<<"BLACK"<<endl;
        }
    }
    return 0;
}
```