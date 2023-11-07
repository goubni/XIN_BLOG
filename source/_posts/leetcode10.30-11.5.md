---
title: 2023 10.30-11.5
excerpt: 每周补题
tags: 刷题
categories: leetcode日常
quicklink: true
date: 2023-11-05 23:00:00

---





# [*275. H 指数 II](https://leetcode.cn/problems/h-index-ii/)

记住二分答案的前提条件是单调性，比如说这题，如果h是3，说明有3篇论文被至少引用了3次，那么也就一定有2篇论文被至少引用了2次，可是4就不一定了，我们只知道3可以，间接的知道比3小的也可以，比3大的不一定。



```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        # 在区间 [left, right] 内询问
        left = 1
        right = len(citations)
        while left <= right:  # 区间不为空
            # 循环不变量：
            # left-1 的回答一定为「是」
            # right+1 的回答一定为「否」
            mid = (left + right) // 2
            # 引用次数最多的 mid 篇论文，引用次数均 >= mid
            if citations[-mid] >= mid:
                left = mid + 1  # 询问范围缩小到 [mid+1, right]
            else:
                right = mid - 1  # 询问范围缩小到 [left, mid-1]
        # 循环结束后 right 等于 left-1，回答一定为「是」
        # 根据循环不变量，right 现在是最大的回答为「是」的数
        return right

```

底蕴不够，理解不深，每次看灵神的题解一知半解。就是记不住





# [2003. 每棵子树内缺失的最小基因值](https://leetcode.cn/problems/smallest-missing-genetic-value-in-each-subtree/)

这一题相当巧妙（灵神tql）

只需要找到基因值为1开始的地方，开始向上dfs

```python
from typing import List


class Solution:
    def smallestMissingValueSubtree(self, parents: List[int], nums: List[int]) -> List[int]:
        n = len(parents)
        ans = [1] * n
        #建图
        g = [[] for _ in range(n)]
        for i in range(1,n):
            g[parents[i]].append(i)
        #得到基因值为1的索引
        node = nums.index(1)
        #设置已经加入进去的基因值
        vis = set()
        def dfs(x):
            vis.add(x)
            for y in g[x]:
                if y not in vis:
                    dfs(y)
        #初始答案是2
        mex = 2
        while node!=-1:
            dfs(node)
            while mex in vis:
                mex += 1
            ans[node] = mex
            node = parents[node]
        return ans

```




