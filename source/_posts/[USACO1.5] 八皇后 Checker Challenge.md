---
title: P1219八皇后CheckerChallenge
excerpt: 无
tags: 回溯
categories: 回溯
quicklink: true
date: 2024-3-17 23:16:00

---

# [USACO1.5] 八皇后 Checker Challenge

## 题目描述

一个如下的 $6 \times 6$ 的跳棋棋盘，有六个棋子被放置在棋盘上，使得每行、每列有且只有一个，每条对角线（包括两条主对角线的所有平行线）上至多有一个棋子。

![](https://cdn.luogu.com.cn/upload/image_hosting/3h71x0yf.png)

上面的布局可以用序列 $2\ 4\ 6\ 1\ 3\ 5$ 来描述，第 $i$ 个数字表示在第 $i$ 行的相应位置有一个棋子，如下：

行号 $1\ 2\ 3\ 4\ 5\ 6$

列号 $2\ 4\ 6\ 1\ 3\ 5$

这只是棋子放置的一个解。请编一个程序找出所有棋子放置的解。  
并把它们以上面的序列方法输出，解按字典顺序排列。  
请输出前 $3$ 个解。最后一行是解的总个数。

## 输入格式

一行一个正整数 $n$，表示棋盘是 $n \times n$ 大小的。

## 输出格式

前三行为前三个解，每个解的两个数字之间用一个空格隔开。第四行只有一个数字，表示解的总数。

## 样例 #1

### 样例输入 #1

```
6
```

### 样例输出 #1

```
2 4 6 1 3 5
3 6 2 5 1 4
4 1 5 2 6 3
4
```

## 提示

【数据范围】  
对于 $100\%$ 的数据，$6 \le n \le 13$。

题目翻译来自NOCOW。

USACO Training Section 1.5

# 思路

主要卡住我的一个点是：如何判断这个点能不能继续搜下去，就是判断这个点会不会和别的点在同一行（这个都好解决，因为是一行一行向下搜，所以不用太担心),在不在同一列，在不在一条对角线上。

在不在同一列，可以开一个数组 $b$ 来判断有没有访问过;在不在一个对角线上怎么做？在对角线上的数都有一个规律：

1. 如果是从左下到右上的对角线，假设两个在一个对角线上的数 $a$ 和 $b$ ，$（a_i表示行 ,a_j表示列 ）$，有 $a_i + a_j = b_i + b_j$，没错，<span style="color:red">就是在一条对角线上的数它们的行列和会相同</span>

2. 如果是左上到右下的对角线有 $n+a_i - a_j = n + b_i-b_j$

那这样就完全有方法解决是否访问这个问题了，然后结合回溯，就能搞定了

```java
package 算法一7搜索;

import java.util.Scanner;

public class P1219八皇后CheckerChallenge { 
    //b代表列是否访问，c代表左下到右上对角线，d代表左上到右下对角线
    static boolean b[] = new boolean[100],c[] = new boolean[100],d[] = new boolean[100];
    static int n,e[] = new int[100],cnt;
    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        n = in.nextInt();
        dfs(1,"");
        System.out.println(cnt);
    }
    public static void dfs(int i,String s){
        if(i > n){
            if(cnt < 3) System.out.println(s.substring(1));
            cnt++;
        }
        for (int j = 1; j <= n ; j++) {
            if(!b[j]&&!c[i+j]&&!d[n+i-j]){
                b[j] = true;
                c[i+j] = true;
                d[n+i-j] = true;
                dfs(i+1,s+(" "+j));
                b[j] = false;
                c[i+j] = false;
                d[n+i-j] = false;
            }
        }
    }
}
```
