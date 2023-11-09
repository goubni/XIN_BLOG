---
title: 最长公共子序列(LCS)
excerpt: 动规O(m*n)
tags: dp
categories: dp题型
quicklink: true
date: 2023-10-30 10:30:00

---

# 概述

最长公共子序列是指两个字符串 s,t,它们最长的有着公共部分的子序列

比如说 s = ABBCBFGHH, t = BCBHHGABBCB,那他们的最长的公共子序列是BCBHH。长度是5，用动态规划可以在O（m*n）的时间内求出最长公共子序列，后面统一称为LCS。求得时候再用一个前驱数组记录就可以求出整个序列

## 状态转移方程

- 先定义f[i][j] 代表字符串s的子串s[0:i]与字符串t的子串t[0:j]的LCS

- 当$s[i]=t[i]$的时候,有 $f[i][j]=f[i-1][j-1]+1$ ,代表前一个子串s[0:i-1]与子串t[0:j-1]的公共部分又可以加1了

- 当$s[i]!=t[j]$的时候,有 $f[i][j]=max(f[i-1][j],f[i][j-1])$ , 此时只能更新前面两种状态

```cpp
#include <iostream>
#include <cstring>
#include <algorithm>
using namespace std;

char s[200] = "ABBCBFGHH";
char t[200] = "BCBHHGABBCB";
int f[201][201];
int m,n;

int main(){
    //求两个串的最长公共子序列 
    m = strlen(s);
    n = strlen(t);
    for(int i=1;i<=m;i++){
        for(int j=1;j<=n;j++){
            if(s[i-1] == t[j-1]){
                f[i][j] = f[i-1][j-1] + 1;
            } else {
                f[i][j] = max(f[i-1][j], f[i][j-1]);
            }
        }
    }
    cout << "最长公共子序列的长度为"<< f[m][n] << endl;
    return 0;
}
```

以上只是简单的求出了LCS的长度,如果要求出LCS序列,那可以再递推的时候,给每个点设置一个前驱,代表这个点是由什么情况转移过来的,这种题型明显只有三种转移的情况

- 由 i-1,j-1转移过来,前驱设为1

- 由 i-1 转移过来,前驱设为2

- 由 j-1 转移过来,前驱设为3

```cpp
#include <iostream>
#include <cstring>
using namespace std;
char s[200] = "ABBCBFGHH";
char t[200] = "BCBHHGABBCB";
int f[201][201];
int p[201][201];
int m,n;

int LCS(){
    m = strlen(s);
    n = strlen(t);
    for(int i=1;i<=m;i++){
        for(int j=1;j<=n;j++){
            if(s[i-1]==t[j-1]){
                f[i][j] = f[i-1][j-1] + 1;
                //1代表前驱是左上方 
                p[i][j] = 1;
            } else {
                if(f[i-1][j] > f[i][j-1]){
                    f[i][j] = f[i-1][j];
                    //2代表前驱是正上方 
                    p[i][j] = 2;
                } else {
                    f[i][j] = f[i][j-1];
                    //3代表前驱是左边 
                    p[i][j] = 3;
                }
            }
        }
    }
    return f[m][n];
}

void getLCS(int len){
    int i=m,j=n;
    char a[len];
    //注意 s数组对应i，t数组对应j 
    while(i>0&&j>0){
        if(p[i][j]==1){
            a[--len]=s[i-1];
            i--;j--;
        } else if(p[i][j]==2){
            i--;
        } else {
            j--;
        }
    }
    cout << "最长公共子序列是";
    for(int i=0;i<f[m][n];i++){
        cout << a[i] << " ";
    }
}

int main(){
    int len = LCS();
    cout << "最长公共子序列的长度是" << len << endl;
    getLCS(len);
    return 0;
}
```

我们有两个字符串s,t,无论是哪个都可以推出这个序列是什么,不过要注意s对应的是i,t对应的是j

前驱数组,我只能说优雅
