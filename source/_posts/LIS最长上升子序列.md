---
title: 线性DP 最长上升子序列(LIS)
excerpt: 动规O(n^2)，二分O(nlogn)
tags: dp
categories: dp题型
quicklink: true
date: 2023-10-25 10:40:00

---

$题目：求一串序列的最长上升子序列的长度$

eg： 输入$a=[5,7,1,9,4,6,2,8,3]$

     输出: 4 ,最长上升子序列是$[1,4,6,8]$

# 方法一：动态规划

## 分析

对于给出的样例，我们可以将它从左往右扫描

- 以5结尾的LIS ：5

- 以7结尾的LIS ：5，7

- 以1结尾的LIS ：1

- 以9结尾的LIS ：5，7，9

- 以4结尾的LIS ：1，4

- 以6结尾的LIS ：1，4，6

- 以2结尾的LIS ：1，2

- 以8结尾的LIS ：1，4，6，8

- 以3结尾的LIS ：1，2，3

在我们从左往右扫描的过程中，我们是怎么得出以xx结尾的LIS，我们是自己计数从头到这个xx的地方的LIS。而且需满足大于前面的数才能确保LIS加1

## 找到转移方程

定义f[i]的含义为以a[i]为结尾字符串的LIS的长度

初始情况下，所有的字符自己构成一个子序列，而且长度为1,即$f[i]=1$

递推式:

如果当前结尾字符比它前面的字符大，说明可以构成一个子序列，再比较一下是当前以i结尾的LIS长度更大还是这个可以构成子序列+1的 长度更大，不要忘记了**f[i]的含义是以a[i]结尾字符串的LIS的长度**

```cpp
if(a[i]>a[j]){
    f[i] = max(f[j]+1,f[i]);
}
```

那么求得最长上升子序列长度得朴素代码

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
int a[] = {5,7,1,9,4,6,2,8,3};
int main(){
    int n = sizeof(a)/sizeof(a[0]);
    int f[n];
    //将数组初始化，全部都为1 
    fill(f,f+n,1);
    int ans = 0;
    for(int i=1;i<=n;i++){
        for(int j=0;j<i;j++){
            if(a[i]>a[j]){
                f[i] = max(f[j]+1,f[i]);
            }
        }
        ans = max(ans,f[i]);
    }
    cout << "这个序列的最长上升子序列的长度是" << ans << endl; 
    return 0; 
}
```

输出结果 4

如果此时我不仅想要你输出这个最长上升子序列得长度，我还要求这个最长上升子序列，那该如何求

我们可以在递推得过程中，在每次LIS变化的时候记录下此时索引，最后利用这个索引向前推，就能推到答案

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
int a[] = {5,7,1,9,4,6,2,8,3};
int main(){
    int n = sizeof(a)/sizeof(a[0]);
    int f[n],end,ans;
    fill(f,f+n,1);
    for(int i=1;i<=n;i++){
        for(int j=0;j<i;j++){
            if(a[i]>a[j]){
                f[i] = max(f[i],f[j]+1);
            }
        }
        //ans变化处做好标记 
        if(f[i]>ans){
            ans = f[i];
            end = i;
        }
    }
    //定义路径数组，长度是LIS的长度 
    int LIS[ans],cnt = ans;
    LIS[--cnt] = a[end];
    for(int i = end-1; i>0;i--){
        if(a[i] < a[end] && f[i]+1 == f[end]){
            LIS[--cnt] = a[i];
            end = i;
        }
    }
    cout << "最长上升子序列是： " ;
    for(int i=0;i<ans;i++){
        cout<< LIS[i] << " ";
    } 
    return 0;

} 
//输出结果 ：最长上升子序列是： 1 4 6 8
```

# 方法二：二分

刚刚的方法是要两重循环$O(n^2)$,而这个方法可以优化到$O(nlogn)$

我们开辟一个数组 b[n],这个数组是用来存最长上升子序列的，在开辟一个变量len，代表LIS的长度的。初始是1

len = 1,b[1] = a[1];

之后我们考虑每一个新添加进来的元素a[i]

- 大则添加：如果加进来的a[i]比b[len]大，就是说a[i]比当前存最长上升子序列的数组b的最后一个元素大，说明可以构成，就将它添加进b里面

- 小则替换：如果加进来的a[i]比b[len]小，就用二分查找找到比a[i]大的那个数，并用a[i]替换掉它。这样可以保证被替换后的序列比替换前的序列更容易**续接**其他的元素，因为对于一个上升子序列来说，它的结尾元素越小，越有利于它续接其他的元素，从而变得更长

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
int a[] = {0,5,7,1,9,4,6,2,8,3};
//用于存LIS 
int b[9];
int len=1; 
int find(int x){
    int l=1,r=len,mid;
    while(l<=r){
        mid=(l+r)/2;
        if(x>b[mid]) l=mid+1;
        else r=mid-1;
    }
    return l;
}


int main(){
    b[1] = a[1];
    for(int i=2;i<=9;i++){
        if(a[i]>b[len]){
            b[++len]=a[i];
        } else {
            int j=find(a[i]);
            b[j]=a[i];
        }
    }
    cout << "最长上升子序列的长度是" << len <<endl;
    for(int i=1;i<=len;i++){
        cout<< b[i] << "->";
    }
    cout << endl;
    return 0;
} 
```

如果想获得最长上升子序列的长度，那就需要加上一些数组来记录信息

- p数组，p[i]代表长度为i的LIS的最后一个元素在a数组的位置

- **parent数组,parent[i]代表parent[i]记录的是原数组a中第i个元素在其所在的最长上升子序列中的前一个元素的位置**。

直接上代码，我也没理解透

```cpp
#include <iostream>
#include <algorithm>
using namespace std;
int a[] = {0,5,7,1,9,4,6,2,8,3};
//用于存LIS 
int b[10];
int p[10], parent[10];
int len=1;
int find(int x){
    int l=1,r=len,mid;
    while(l<=r){
        mid=(l+r)/2;
        if(x>b[mid]) l=mid+1;
        else r=mid-1;
    }
    return l;
}

int main(){
    b[1] = a[1];
    p[1] = 1;
    for(int i=2;i<=9;i++){
        if(a[i]>b[len]){
            b[++len]=a[i];
            p[len] = i;
            parent[i] = p[len-1];
        } else {
            int j=find(a[i]);
            b[j]=a[i];
            p[j] = i;
            parent[i] = p[j-1];
        }
    }
    cout << "最长上升子序列的长度是" << len <<endl;
    int k = p[len];
    int stack[10], top = -1;
    while(k != 0) {
        stack[++top] = a[k];
        k = parent[k];
    }
    while(top >= 0) {
        cout << stack[top--] << "->";
    }
    cout << endl;
    return 0;
}
```

<br>

> 最长上升子序列有两种解法，一种动态规划$O(n^2)$，一种二分$O(nlogn)$。



模板题练手：

[300. 最长递增子序列 - 力扣（LeetCode）](https://leetcode.cn/problems/longest-increasing-subsequence/description/)

[B3637 最长上升子序列 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/B3637)

两种写法都要掌握

[记录详情 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/record/131577741)，这一题用二分加上暴力模拟，可以过三个例子，不过也可以很好的练手

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
#define N 100010
using namespace std;

int find(int x,vector<int> a,int len,vector<int> b){
    int l=1,r=len,mid;
    while(l<=r){
        mid=(l+r)/2;
        if(x>b[mid]) l=mid+1;
        else r=mid-1;
    }
    return l;
}

int f(vector<int> a, int MAX){
    vector<int> b(MAX+1);
    int len=1;
    b[1]=a[0];
    for(int i=1;i<=MAX;i++){
        if(a[i] > b[len]){
            b[++len]=a[i];
        } else {
            int j=find(a[i],a,len,b);
            b[j]=a[i];
        }
    }
    return len;
}

int main(){
    int n;
    cin >> n;
    vector<int> a(n);
    for(int i=0;i<n;i++){
        int t;
        cin >> t;
        a.insert(a.begin()+t,i+1);
        if(i==0){
            cout << 1 << endl;
        } else {
            cout << f(a,i) << endl;
        }
    }
    return 0;
}
```
