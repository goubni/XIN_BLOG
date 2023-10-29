---
title: 2023 10.23-10.29
excerpt: 每周补题
tags: 刷题
categories: leetcode日常
quicklink: true
date: 2023-10-29 23:00:00
---



# *[1155. 掷骰子等于目标和的方法数](https://leetcode.cn/problems/number-of-dice-rolls-with-target-sum/)

看到题目这尿性，我就知道是动态规划，后面有大佬指点说可以用容斥原理+组合可以做，这种方法现在实力还不够，先用动态规划解决，这个动态规划的转移方程又不是我自己想出来的，郁闷。

1. 先定义f[i][j]代表i枚骰子抛出来的和为j的情况

2. $f[i][j] = f[i-1][j-1]+f[i-1][j-2]+f[i-1][j-3]+...+f[i-k]$
   
   - f[i-1][j-1]就代表当骰子是i-1枚的时候，抛出了j-1的情况，此时再抛一个1，就能满足情况
   
   - f[i-1][j-2]就代表当骰子是i-2枚的时候，抛出了j-2的情况，此时再抛一个2，就能满足情况
   
   - f[i-1][j-3]就代表当骰子是i-3枚的时候，抛出了j-3的情况，此时再抛一个3，就能满足情况
   
   - f[i-1][j-k]就代表当骰子是i-1枚的时候，抛出了j-k的情况，此时再抛一个k，就能满足情况
   
   - 依此类推，再将它们相加就可将当前情况(i枚骰子)由前一种情况(i-1枚骰子)转移过来

3. 这个状态转移方程还有一个特点，就是得满足当前和target>=骰子面数才成立

```python
class Solution:
    def numRollsToTarget(self, n: int, k: int, target: int) -> int:
        f = [[0]*(target+1) for _ in range(n+1)]
        mod = 10**9+7
        f[0][0]=1
        for i in range(1,n+1):
            for j in range(target+1):
                for m in range(1,k+1):
                    if j >= m:
                        f[i][j] = (f[i][j] + f[i-1][j-m])%mod
        return f[n][target]
```



时间复杂度$O(n*k*target)$





## 空间优化





## 灵神题解







# [2698. 求一个整数的惩罚数](https://leetcode.cn/problems/find-the-punishment-number-of-an-integer/)

这题也是真牛魔，第一次在周赛上遇到，回溯没写出来，看了灵神的题解后，时隔6个月，又不回了。。。



```python
Pre = [0]*(1001)
for i in range(1,1001):
    s = str(i*i)
    n = len(s)
    def dfs(start,sum):
        if start == n:
            return sum == i
        y = 0
        for j in range(start,n):
            y = y*10 + int(s[j])
            if dfs(j+1,sum+y):
                return True
        return False
    if dfs(0,0):
        Pre[i] = i*i + Pre[i-1]
    else:
        Pre[i] = Pre[i-1]
class Solution:
    def punishmentNumber(self, n: int) -> int:
        return Pre[n]





```



唉，真是一生之敌。臭递归，总写写不好





# [1465. 切割后面积最大的蛋糕](https://leetcode.cn/problems/maximum-area-of-a-piece-of-cake-after-horizontal-and-vertical-cuts/)

这一题考验对数学的感觉吧，反正我是一下就想到了，长取最大，宽取最大，相乘就是答案

```python
from typing import List


class Solution:
    def maxArea(self, h: int, w: int, horizontalCuts: List[int], verticalCuts: List[int]) -> int:
        def get(s):
            s.sort()
            v = 0
            t = 0
            for x in s:
                v = max(v,x-t)
                t = x
            return v
        horizontalCuts.append(h)
        verticalCuts.append(w)
        return get(horizontalCuts)*get(verticalCuts)%int(1e9 + 7)


```





# [274. H 指数](https://leetcode.cn/problems/h-index/)



## 暴力

这题一开始只想到了暴力，枚举h，时间复杂度是O(n^2)

```python
from typing import List


class Solution:
    def hIndex(self, citations: List[int]) -> int:
        def func(h):
            c = 0
            for x in citations:
                if x >= h:
                    c += 1
            return c >= h

        n = len(citations)
        ans = 0
        for i in range(1, n+1):
            if func(i):
                ans = max(ans,i)
        return ans
```

针对每一个指数，都去看看他能否满足条件，满足就更新答案





## 排序

先排个序，然后从后向前遍历，h可以从0开始加，如果当前数比h大，说明至少找到了一篇

被至少引用h次的论文，那么h++，直到h不能再增加为止

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        n = len(citations)
        i,h = n-1,0
        citations.sort()
        while i>=0 and citations[i]>h:
            h+=1
            i-=1
        return h
```

O(logn)



# *计数排序

上述解法的主要耗时就是在排序这一块，排好序之后，需要从前往后依次判断，如果用计数排序，就可将复杂度降到O(n)

```python
class Solution:
    def hIndex(self, citations: List[int]) -> int:
        n = len(citations); tot = 0
        counter = [0] * (n+1)
        for c in citations:
            if c >= n:
                counter[n] += 1
            else:
                counter[c] += 1
        for i in range(n, -1, -1):
            tot += counter[i]
            if tot >= i:
                return i
        return 0


```







> 因为这周实在太忙，准备软考，连周赛都没打，这个补题也有点敷衍了😵😅
> 
> 周赛后面得补上
