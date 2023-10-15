---
title: 2023 10.9-10.15
excerpt: 每周补题
quicklink: true
tags: 刷题
categories: leetcode日常
date: 2023-10-15 19:00:00
---

## 2023.10.9

[2578. 最小和分割](https://leetcode.cn/problems/split-with-minimum-sum/)

一看这题我确实想复杂了，就是那下贪心没有想到，一个排好序的数 12345，从里面抽出来两个数的和要求最小，那就是得越高位越小，肯定以1开头要组成一个数，以2开头要组成一个数才能做到最小。这题还真得看感觉能不能品到这一下：135 + 24

```python
class Solution:
    def splitNum(self, num: int) -> int:
       s = ''.join(sorted(str(num)))
       num1, num2 = int(s[::2]), int(s[1::2])
       return num1 + num2
```



## 2023.10.10

[2731. 移动机器人](https://leetcode.cn/problems/movement-of-robots/)

这题最离谱，记录上显示我周赛ac过这道题，然后想半天没又做出来，去看以前的记录，看完后：这真是我能写出来的代码嘛😑，和题解都不是一个思路的。。。



首先先来看下官方解答，我也从中学到了这个技巧：

> 当要求一个序列 [2,4,6,8,19,20] 它们两两之间的差值，并且求这些差值的和

采用贡献的方法：这里面有6个数，从第二个数开始遍历(此时i=1)，求出与前一个数的差，4-2=2，也就是说，这个元素4前面有1个数给4-2 做贡献，4后面有6-1个数再给4-2做贡献。i * (n - i) 就是这个 4 - 2 能为答案提供的方案。



```python
class Solution:
    def sumDistance(self, nums: List[int], s: str, d: int) -> int:
       mod = 10**9 + 7
       n = len(nums)
       pos = [nums[i] - d if s[i] == 'L' else nums[i] + d for i in range(n)]
       pos.sort()
       return sum([pos[i] - pos[i - 1] * i * (n - i) for i in range(1,n))
```



## 2023.10.11

[2512. 奖励最顶尖的 K 名学生](https://leetcode.cn/problems/reward-top-k-students/)

我这题是真傻逼了，我就是傻逼，写了个trie去实现，脑子没转过来，用哈希表去实现查找都是O(1),而trie是O(logn),这题没啥好说，单纯模拟题



O（nlogn）  用Trie实现，哈希表就不写了

```py
class Trie:
    def __init__(self):
        self.tree = {}
    def insert(self, s):
        node = self.tree
        for x in s:
            if x not in node.keys():
                node[x] = {}
            node = node[x]
        node['#'] = '#'
    def search(self, s):
        node = self.tree
        for x in s:
            if x in node.keys():
                node = node[x]
            else:
                return False
        return '#' in node.keys()

class Solution:
    def topStudents(self, positive_feedback: List[str], negative_feedback: List[str], report: List[str], student_id: List[int], k: int) -> List[int]:
        pos_tree = Trie()
        neg_tree = Trie()
        for x in positive_feedback:
            pos_tree.insert(x)
        for x in negative_feedback:
            neg_tree.insert(x)
        ans = []
        for x in report:
            t = 0
            for y in x.split():
                if pos_tree.search(y):
                    t += 3
                    continue
                if neg_tree.search(y):
                    t -= 1
            ans.append(t)
        sor = []
        for i in range(len(report)):
            sor.append([ans[i], student_id[i]])
        ans = sorted(sor,key= lambda x:(x[0],-x[1]),reverse=True)[:k]
        return [x[1] for x in ans]



```



## 2023.10.12

[2562. 找出数组的串联值](https://leetcode.cn/problems/find-the-array-concatenation-value/)

没啥好说，模拟题

```python
class Solution:
    def findTheArrayConcVal(self, nums: List[int]) -> int:
        S = 0
        n = len(nums)
        for i in range(n // 2):
            temp = str(nums[i]) + str(nums[n-i-1])
            S += int(temp)
        return S if n % 2 == 0 else S + nums[n // 2]
```



## ***2023.10.13**

[1488. 避免洪水泛滥](https://leetcode.cn/problems/avoid-flood-in-the-city/)

这题可有意思了，唤醒了我的二分记忆，差点把这个东西忘记了（~~太久没刷题~~），

我真笨，再看又理解不来了

```python
from typing import List
from sortedcontainers import SortedList

class Solution:
    def avoidFlood(self, rains: List[int]) -> List[int]:
        ans = [1] * len(rains)
        mp = {}
        st = SortedList()
        for i, x in enumerate(rains):
            if x == 0:
                st.add(i)
            else:
                ans[i] = -1
                if x in mp:
           
                    it = st.bisect(mp[x])
                    if it == len(st):
                        return []
                    ans[st[it]] = x
                    st.discard(st[it])
                mp[x] = i
        return ans
```

服了，这狗题目把我搞晕了，先挂着



## 2023.10.14

[136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)

立马想到异或异或异或异或异或异或

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ret = 0;
        for (auto e: nums) ret ^= e;
        return ret;
    }
};

```



## 2023.10.15

[137. 只出现一次的数字 II](https://leetcode.cn/problems/single-number-ii/)

天才们创造了世界

这题是上面那题的变式，由于最优解实在太复杂，涉及到数字电路的知识，于是退而求O(nlogU)的做法，也很强了，logU最大是32



将数组里所有的数都变成二进制的话，看数据范围知道最多只有32位，那我们就从第一位开始，将所有数的这一位都加起来，因为这一位只有可能是0或1，而那些相同的三个数它们加起来就只有3和0两种可能，不管哪种都可以被3整除(0%3==0)

>  因此可以得出判断条件：当这些总和可以被3整除的时候，说明那个答案的这一位   是0，反之就是1。

```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int ans = 0;
        for (int i = 0; i < 32;++i) {
            int total = 0;
            for (int num: nums) {
                total += ((num >> i) & 1);
            }
            if (total % 3) {
                //将ans的第(i+1)位置成1
                ans |= (1 << i);
            }
        }
        return ans;
    }
};

```



学到的小技巧：

> 获得一个数的二进制形式的第i位（从右向左）: (x >> (i - 1)) & 1

> 将一个数的二进制的第i位置成1：x |= 1 << (i - 1)





刷题又占时间性价比又低，上瘾了，废


