---
title: 双指针单调栈一些写法
date: 2025-07-25 13:27:57
tags: acm培训
cover: /img/02.jpg
---
## 双指针单调栈一些写法
### 双指针
双指针本质其实是对两层for循环的一些优化，我们知道两层循环的时间复杂度大概是在o（n^2），而双指针往往只用了一层遍历就达到了目的，时间复杂度在o（n）。
双指针的写法主要依据题目而定，但往往题目对于答案有多个限制要求，标准写法往往是通过for循环或者while循环使右指针有一个从左到右的遍历，然后左指针在满足条件的情况下缩短与右指针的距离。
举一个例子。
给定一个正整数数组 nums 和一个目标值 s，找到 和 ≥ s 的 最短连续子数组的长度，如果不存在这样的子数组，返回 0。
输入：
s = 7
nums = [2,3,1,2,4,3]

输出：
2

解释：
子数组 [4,3] 的和是 7，长度为 2，是满足条件的最短子数组。
我们不难发现这道题有几个限制要求，一个是 ≥ s 另外一个是最短连续子序列，那么内层循环要做的就是在 ≥ s的情况下缩短子序列长度，下面是示例代码。
```cpp
int minSubArrayLen(int s, vector<int>& nums) {
    int n = nums.size();
    int left = 0, sum = 0, min_len = INT_MAX;

    for (int right = 0; right < n; ++right) {
        sum += nums[right];

        while (sum >= s) {
            min_len = min(min_len, right - left + 1);
            sum -= nums[left];
            ++left;
        }
    }

    return min_len == INT_MAX ? 0 : min_len;
}
```
### 单调栈
单调栈往往解决的问题是数组中每一个数的下一个最大或最小的数，也可以是上一个，通过一个反向遍历，只单调递增或单调递减的栈stack来实现功能。
举个例子。给定一个数组 a[]，对于每个位置 i，找出右边第一个比 a[i] 大的元素，返回其下标（没有则为 -1）。
```cpp
#include <iostream>
#include <stack>
#include <vector>
using namespace std;

int main() {
    vector<int> a = {2, 1, 5, 3, 6, 4};
    int n = a.size();
    vector<int> ans(n, -1); // 记录答案，默认都为 -1
    stack<int> st; // 存下标

    for (int i = 0; i < n; ++i) {
        while (!st.empty() && a[i] > a[st.top()]) {
            ans[st.top()] = i;
            st.pop();
        }
        st.push(i);
    }

    for (int i = 0; i < n; ++i)
        cout << "右边第一个比 a[" << i << "]=" << a[i] << " 大的是 a[" << ans[i] << "]" << endl;

    return 0;
}
```
需要注意的是单调栈往往存的是下标，通过一个while循环批量踢出大于或小于每次循环中的a[i]元素，将答案记录在ans数组中输出。