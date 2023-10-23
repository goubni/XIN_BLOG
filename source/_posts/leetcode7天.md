---
title: 2023 10.16-10.22
excerpt: 每周补题
tags: 刷题
categories: leetcode日常
quicklink: true
date: 2023-10-22 18:00:00

---

# [260. 只出现一次的数字 III](https://leetcode.cn/problems/single-number-iii/)

要实现常量额外空间还是有一定难度的

1. 可以直接先得到一个异或和 xor。这个异或和就是那两个仅仅出现一次的数字的异或和

2. 再得到这个异或和的最后一是1的数，用lowbit得出(x&(-x))

3. 再遍历一次数组，用lowbit与他们区分，是0的分成一个数组，是1的分成另一个数组，分别求数组里的异或和，就是答案

注意：lowbit(x)之后，去&一个数，就是得到这个数在这一位的值，只有两种可能0或1

```python
class Solution:
    def singleNumber(self, nums: List[int]) -> List[int]:
        xor = 0
        for x in nums:
            xor ^= x
        lowbit = xor&(-xor)
        ans = [0,0]
        for x in nums:
            if x&lowbit:
                ans[0] ^= x
            else:
                ans[1] ^= x
        return ans
```

<br>

# [2652. 倍数求和](https://leetcode.cn/problems/sum-multiples/)

给你一个正整数 `n` ，请你计算在 `[1，n]` 范围内能被 `3`、`5`、`7` 整除的所有整数之和。

返回一个整数，用于表示给定范围内所有满足约束条件的数字之和。

开眼了，眼前一新

**容斥原理**

在区间 [1,n]内能被m整除的整数，从小的到大排序后成为一个等差数列，和为 `（首项+某项）*项数/2`，首项自然是m，项数自然是$[\frac{n}{m}]$,末项自然是$[\frac{n}{m}]*m$

于是就得出函数 $f(n,m)=\frac{(m+m*[\frac{n}{m}])*[\frac{n}{m}]}{2}$

根据容斥原理，在[1,n]内，能被3、5、7整除的整数之后为：

$f(n,3)+f(n,5)+f(n,7)-f(n,3*5)-f(n,3*7)-f(n,5*7)+f(n,3*5*7)$

```python
class Solution:
    def sumOfMultiples(self, n: int) -> int:
        def f(n,m):
            return (m+n//m*m)*(n//m)//2;
        return f(n, 3) + f(n, 5) + f(n, 7) - f(n, 3 * 5) - f(n, 3 * 7) - f(n, 5 * 7) + f(n, 3 * 5 * 7)
```

<br>

# [2530. 执行 K 次操作后的最大分数](https://leetcode.cn/problems/maximal-score-after-applying-k-operations/)

这题无非就是用堆，不过大佬们写的比我写的好看，优雅

1. 以后看到什么ceil(nums[i]/b)，向上取整这种，可以用 (nums[i] + b-1) / b 来达到效果

2. 记住heapq.heapify是建最小堆的函数。

3. 熟练使用heapq模块

```python
class Solution:
    def maxKelements(self, nums: List[int], k: int) -> int:
        q = [-x for x in nums]
        heapify(q)
        ans  = 0
        for _ in range(k):
            x = heappop(q)
            ans += -x
            heappush(q,-((-x+2)//3))
        return ans
```

<br>

# [1726. 同积元组](https://leetcode.cn/problems/tuple-with-same-product/)

是一个组合数的问题，预处理出所有的两两乘积，再计数，只要不是只有一个元素，都可以构成同积元组，用组合数求出有几种方案，之后再用方案数乘8，再累加就是答案了。

ps:python中的comb是用与求组合数比较小的时候的函数

```python
from collections import Counter
from math import comb
from typing import List


class Solution:
    def tupleSameProduct(self, nums: List[int]) -> int:
        n = len(nums)
        lst = []
        for i in range(n):
            for j in range(i + 1,n):
                lst.append(nums[i] * nums[j])
        return sum([comb(v, 2) * 8 for k,v in Counter(lst).items() if v != 1])
```

<br>

# [2525. 根据规则将箱子分类](https://leetcode.cn/problems/categorize-box-according-to-criteria/)

太简单，没啥好说

<br>

# [2316. 统计无向图中无法互相到达点对数](https://leetcode.cn/problems/count-unreachable-pairs-of-nodes-in-an-undirected-graph/)

看到一个图是否联通，我立马想到了并查集（~~太久没写板子忘了~~）和深搜(~~蒟蒻写炸了~~)，然后就没有然后了。

复习一下并查集的板子

```python
class Union:
    def __init__(self,n:int):
        self.fa = [i for i in range(n)]
        #记录每颗树有多少个节点，初始每个结点的祖宗结点是他自己，就一个
        self.size = [1] * n
    def find(self,x:int):
        if self.fa[x] == x:
            return x
        self.fa[x] = find(self.fa[x])
        return self.fa[x]
    def merge(self,x:int.y:int):
        fx = self.find(x)
        fy = self.find(y)
        if fx != fy:
            if self.size[fx] > self.size[fy]:
                self.fa[fy] = fx
                self.size[fx] += self.size[fy]
            else:
                self.fa[fx] = fy
                self.size[fy] += self.size[fx]
```

看第二种做法——深搜

每次写深搜都头痛，写完后还不确定自己有没有对，好难啊

我们有一个结点i，当我们想要搜索从这个结点x开始有哪些分量和他联通的时候，以下代码是通用的

当你想要计数有几个结点时

```python
vis = [False] * n
def dfs(i:int)->int:
    vis[i] = True
    cnt = 1
    for x in graph[i]:
        if not vis[x]:
            cnt += dfs(i)
    return cnt
```

```python
class Solution:
    def countPairs(self, n: int, edges: List[List[int]]) -> int:
        graph = [[] for i in range(n)]
        for u, v in edges:
            graph[u].append(v)
            graph[v].append(u)
        vis = [False] * n
        def dfs(x:int)->int:
            vis[x] = True
            cnt = 1
            for x in graph[x]:
                if not vis[x]:
                    cnt += dfs(x)
            return cnt
        ans = 0
        for i in range(n):
            if not vis[i]:
                cnt = dfs(i)
                # 每一个联通圈里的结点数的贡献是这么多
                ans += cnt*(n - cnt)
        return ans//2
```

<br>

# <span style="color:red">*[1402. 做菜顺序](https://leetcode.cn/problems/reducing-dishes/)</span>

本周唯一的一道困难题

我闻着这个题目的味道就知道是用dp，在草稿纸上圈圈划划好久也没有推出转移方程，一看答案就通了😭😭😭😭。鬼题目,其实就是一个背包

1. 首先，肯定是越大的数排在越后面最后，这样可以ans是最大的

2. 然后就是最难想的一步 dp[i][j] 代表在$i$道菜里面选出$j$道菜的最大的like-time系数总和

3. 肯定是分两种情况，一种是由前一种不选的转移过来和前一种选的转移过来
   
   - 不选的话 $dp[i][j] = max(dp[i-1][j],dp[i][j])$
   
   - 选的话     $dp[i][j]=max(dp[i-1][j-1]+satisfaction[i-1]*j,dp[i][j])$

```python
class Solution:
    def maxSatisfaction(self, satisfaction: List[int]) -> int:
        n = len(satisfaction)
        satisfaction.sort()
        f = [[0]*(n+1) for _ in range(n+1)]
        res = 0
        for i in range(1,n+1):
            for j in range(1,i+1):
                #先计算出选这道菜的like-time有多少
                f[i][j] = f[i-1][j-1] + satisfaction[i-1]*j
                #防止越界，因为是不选的情况，会有f[i-1][j],那j就必须小于i
                if j < i:
                    f[i][j] = max(f[i][j],f[i-1][j])
                res = max(res,f[i][j])
        return res
```

2. 第二种贪心思路
   
    暂略，忙，挖个坑
