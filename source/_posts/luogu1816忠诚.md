---
title: Luogu1816 线段树区间最小值小变体
excerpt: 区间最小值
tags: 线段树
categories: 线段树系列
quicklink: true
date: 2023-12-21 18:16:00

---



# 忠诚

## 题目描述

老管家是一个聪明能干的人。他为财主工作了整整  $10$ 年。财主为了让自已账目更加清楚，要求管家每天记  $k$ 次账。由于管家聪明能干，因而管家总是让财主十分满意。但是由于一些人的挑拨，财主还是对管家产生了怀疑。于是他决定用一种特别的方法来判断管家的忠诚，他把每次的账目按  $1, 2, 3 \ldots$ 编号，然后不定时的问管家问题，问题是这样的：在   $a$ 到  $b$ 号账中最少的一笔是多少？为了让管家没时间作假，他总是一次问多个问题。

## 输入格式

输入中第一行有两个数  $m, n$，表示有  $m$ 笔账  $n$ 表示有  $n$ 个问题。

第二行为  $m$ 个数，分别是账目的钱数。

后面  $n$ 行分别是  $n$ 个问题，每行有   $2$ 个数字说明开始结束的账目编号。

## 输出格式

在一行中输出每个问题的答案，以一个空格分割。

## 样例 #1

### 样例输入 #1

```
10 3
1 2 3 4 5 6 7 8 9 10
2 7
3 9
1 10
```

### 样例输出 #1

```
2 3 1
```

## 提示

对于 $100\%$ 的数据，$m \leq 10^5$，$n \leq 10^5$。



# 思路



这一题明显是求出区间的最小值，一眼线段树了，重点就是如何在线段树模板的基础上更新成区间最小值就可以了，只需要在更新的地方依次换就可以了，比如

```cpp
void build(int p, int l, int r) {
    tr[p].l = l;
    tr[p].r = r;
    if (l == r) {
        tr[p].sum = w[l];
        return;
    }
    int m = (l + r) >> 1;
    build(lc, l, m);
    build(rc, m + 1, r);
    tr[p].sum = tr[lc].sum + tr[rc].sum;
}
```

<span style="color:red">我们只需要在求和的地方改成求最小值就可以了,即</span>



```cpp
void build(int p, int l, int r) {
    tr[p].l = l;
    tr[p].r = r;
    if (l == r) {
        tr[p].min_v = w[l];
        return;
    }
    int m = (l + r) >> 1;
    build(lc, l, m);
    build(rc, m + 1, r);
    tr[p].min_v = min(tr[lc].min_v, tr[rc].min_v);
}
```



查询也是如此，依次更新最小值即可

```cpp
int interval_query(int p,int x,int y){
	if(x<=tr[p].l&&tr[p].r<=y){
		return tr[p].min_v;
	}
	int m = tr[p].l+tr[p].r>>1;
	int v = MAX;
	if(x<=m) v = min(v,interval_query(lc,x,y));
	if(y>m) v = min(v,interval_query(rc,x,y));
	return v;
}
```



大体思路就是这样，没什么好说的，上AC代码

```cpp
#include <bits/stdc++.h>
using namespace std;
#define MAX 1e9
#define N 100005
#define lc p<<1
#define rc p<<1|1
int n, w[N],m,a,b;
struct node {
    int l, r, min_v;
} tr[N * 4];

void build(int p, int l, int r) {
    tr[p].l = l;
    tr[p].r = r;
    if (l == r) {
        tr[p].min_v = w[l];
        return;
    }
    int m = (l + r) >> 1;
    build(lc, l, m);
    build(rc, m + 1, r);
    tr[p].min_v = min(tr[lc].min_v, tr[rc].min_v);
}
int interval_query(int p,int x,int y){
	if(x<=tr[p].l&&tr[p].r<=y){
		return tr[p].min_v;
	}
	int m = tr[p].l+tr[p].r>>1;
	int v = MAX;
	if(x<=m) v = min(v,interval_query(lc,x,y));
	if(y>m) v = min(v,interval_query(rc,x,y));
	return v;
}

int main(){
	cin >> n >> m;
	for(int i=1;i<=n;i++) cin >> w[i];
	build(1,1,n);
	while(m--){
		cin >> a >> b;
		cout << interval_query(1,a,b) << " ";
	}
	return 0;
}
```



没了，狗期末考试，快把我逼成前端工程师了😵
