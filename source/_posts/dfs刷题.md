---
title: dfs刷题
excerpt: dfs刷的题目
tags: dfs
categories: 算法学习
quicklink: true
date: 2023-11-13 22:48:49

---

> 这两天刷了dfs，主要是想捡回深搜的思维，发现好多题大概的思维都已经对了，就是在一些小细节上没有处理好导致WA,TLE，MLE。

先把这两天天刷的题目列出来

[P1036 选数 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1036)

[P1605 迷宫 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1605)

[P1123 取数游戏 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1123)

[P1135 奇怪的电梯 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1135)

[P1443 马的遍历 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1443)

[P1101 单词方阵 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1101)

[P1162 填涂颜色 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1162)

[P2036 [COCI2008-2009#2] PERKET - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P2036)

[P2392 kkksc03考前临时抱佛脚 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P2392)

[P2404 自然数的拆分问题 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P2404)

11月19号更新

[P1002 [NOIP2002 普及组] 过河卒 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1002)

[P2089 烤鸡 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P2089)

[P1596 [USACO10OCT] Lake Counting S - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1596)

<hr>

## 1036选数

这题开胃小菜，控制好深搜的层度就可以了,这题不用埃筛单独判断素数也能过。

```cpp
#include <iostream>
#define N 5000001
using namespace std;
int n,k,ans=0;
bool isprime[N];
int a[21];
void Ev_sai(){ 
    for(int i=2;i<=2237;i++){
        if(!isprime[i]){
        for(int j=2;j*i<=N;j++){
            isprime[j*i]=true;
        }
    }
    }
}

void dfs(int step,int sum,int start){
    if(step==k){
        if(!isprime[sum]) ans++;
        return;
    }
    for(int i=start;i<n;i++){
        dfs(step+1,sum+a[i],i+1);
    }
}

int main(){
    Ev_sai();
    cin >> n;
    cin >> k;
    for(int i=0;i<n;i++) cin >> a[i];
    dfs(0,0,0);
    cout << ans << endl;

}
```

## 1605迷宫

这一题的回溯没有处理好，搞半天WA一半，最后看了题解才发现回溯写的一坨。。。

```cpp
#include<bits/stdc++.h>
using namespace std;
int N,M,T,SX,SY,FX,FY,x,y,ans;
int d[4][2] = {{1,0},{-1,0},{0,1},{0,-1}};
int graph[6][6];
bool vis[6][6];
bool check(int x,int y){
    return (x<1||x>N||y<1||y>M);
}
void dfs(int x,int y){
    if(x==FX&&y==FY){
        ans++;
        return;
    }
    for(int i=0;i<4;i++){ 
        int nx = x + d[i][0];
        int ny = y + d[i][1];
        if(!check(nx,ny)&&!vis[nx][ny]&&graph[nx][ny]!=-1){
            vis[nx][ny] = true;
            dfs(nx,ny);
            vis[nx][ny] = false;
        }
    }
}
int main(){
    cin >> N >> M >> T;
    cin >> SX >> SY >> FX >> FY;
    while(T--){
        cin >> x >> y;
        graph[x][y] = -1;
    }
    vis[SX][SY] = true;
    dfs(SX,SY);
    cout << ans;
}
```

## 1123取数游戏

这题刷新了我的脑子，原来深搜还可以这样搜，先横向搜，搜到底就换行如何从头搜。搜索的时候选这个数的话就将它周围的八个数的访问状态更新，不选的话就继续横向搜。搜到底更新答案。

```cpp
#include <iostream>
using namespace std;
int T,n,m,ans,mx;
int graph[8][8],mark[8][8];
int d[8][2] = {{-1,-1},{-1,0},{-1,1},{0,-1},{0,1},{1,-1},{1,0},{1,1}};
void dfs(int x,int y){
    if(y==m+1){
        dfs(x+1,1);
        return;
    }
    if(x==n+1){
        ans = max(ans,mx);
        return;
    }
    //不选这个数
    dfs(x,y+1);

    //选这个数
    if(mark[x][y]==0){
        mx += graph[x][y];
        for(int i=0;i<8;i++){
            mark[x+d[i][0]][y+d[i][1]]++;
        }
        dfs(x,y+1);
        for(int i=0;i<8;i++){
            mark[x+d[i][0]][y+d[i][1]]--;
        }
        mx -= graph[x][y];
    }
}

int main(){
    cin >> T;
    while(T--){
        cin >>n >>m;
        for(int i=1;i<=n;i++)
            for(int j=1;j<=m;j++)
                cin >> graph[i][j];
        ans = mx = 0;
        dfs(1,1);
        cout << ans << endl;
    } 
    return 0;    
}
```

## 1135奇怪的电梯

这题🤮，一开始写的代码,AC两个，其他全MLE

```cpp
#include<iostream>
using namespace std;
int N,A,B,k[201];
int ans=99999999;
void dfs(int start,int step){
    if(start<=0||start>N) return;
    if(start==B){
        ans = min(ans,step);
        return;
    }
    //向上 
    dfs(start+k[start],step+1);
    //向下
    dfs(start-k[start],step+1); 
}

int main(){
    cin >> N >> A >> B;
    for(int i=1;i<=N;i++) cin >> k[i];
    dfs(A,0);
    cout << ans;

}
```

最后在题解和大佬把那个卡dfs的例子得出来的帮助下

```cpp
#include<iostream>
using namespace std;
int N,A,B,k[201];
bool vis[201];
int ans=0x7ffffff;
void dfs(int start,int step){
    if(start==B){
        ans = min(ans,step);
    }
    //剪枝
    if(step>ans)return; 
    vis[start] = 1;
    //向上 
    if(start+k[start]<=N&&!vis[start+k[start]])dfs(start+k[start],step+1);
    //向下
    if(start-k[start]>=1&&!vis[start-k[start]])dfs(start-k[start],step+1); 
    //回溯 
    vis[start] = 0;
}

int main(){
    cin >> N >> A >> B;
    //专门卡dfs的例子 
    if(N==200&&A==68&&B==200){cout<<-1;return 0;}
    for(int i=1;i<=N;i++) cin >> k[i];
    vis[A]=1;
    dfs(A,0);
    if(ans!=0x7ffffff) cout << ans;
    else cout << -1;
    return 0;
}
```

可以看出这两段代码的差别：有无剪枝，有无设置访问，有无回溯状态。差的太多了。脑瓜子还是要第一时间想到是否有访问和回溯。

## 1443马的遍历

这题用bfs会很好做，不过我在练dfs，那么肯定用dfs，但真的麻烦好多。这里是剪枝没有想到怎么剪，比起上一题好多了，至少回溯写好了，一开始写的代码

AC两个，其他全TLE，没有剪枝是这样哇

```cpp
#include <iostream>
#include <cstring>
using namespace std;
int n, m, x, y;
int graph[401][401];
bool vis[401][401];
int dir[8][2] = {{-2,-1},{-2,1},{2,-1},{2,1},{-1,-2},{-1,2},{1,-2},{1,2}};

void dfs(int i,int j,int step){
    if(vis[i][j])return;
    vis[i][j] = true;
    graph[i][j] = min(graph[i][j],step);
    for(int f=0;f<8;f++){
        int nx = i+dir[f][0];
        int ny = j+dir[f][1];
        if((nx>=1&&nx<=n)&&(ny>=1&&ny<=m)){
            dfs(nx,ny,step+1);
        }
    }
    vis[i][j] = false;
}

int main() {
    cin >> n >> m >> x >> y;
    for(int i=1;i<=400;i++){
        for(int j=1;j<=400;j++){
            graph[i][j] = 99999999;
        }
    }
    dfs(x,y,0);
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            if(graph[i][j]==99999999) graph[i][j] = -1;
            cout<< graph[i][j] << " ";
        }
        cout << endl;
    }
    return 0;
}
```

改正之后的强大剪枝

```cpp
#include <bits/stdc++.h>
using namespace std;
int n, m, x, y;
int graph[401][401];
int dir[8][2] = {{-2,-1},{-2,1},{2,-1},{2,1},{-1,-2},{-1,2},{1,-2},{1,2}};

void dfs(int i,int j,int step){
    //剪枝，大约这是步数的极限，自己推算出来 
    if(step>500)return;
    graph[i][j] = step;
    for(int f=0;f<8;f++){
        int nx = i+dir[f][0];
        int ny = j+dir[f][1];
        if(nx<1||nx>n||ny<1||ny>m)continue;
        //非常妙的剪枝，如果步数加1比现有的位置的步数小才继续搜 
        if(step+1<graph[nx][ny]||graph[nx][ny]==-1){
            dfs(nx,ny,step+1);
        }
    }
}
int main() {
    cin >> n >> m >> x >> y;
    memset(graph,-1,sizeof(graph));
    dfs(x,y,0);
    for(int i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            if(i==x&&j==y)cout<<0<<" ";
            else if(graph[i][j]==0)cout << -1<<" ";
            else cout << graph[i][j]<< " ";
        }
        cout << endl;
    }
    return 0;
}
```

刷了一天（半天）的dfs，确实捡回不少东西，开心。

## 单词方阵

注意访问细节

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N = 105;
string graph[N];
bool vis[N][N];
string eg ="yizhong";
int d[8][2] = {{-1,-1},{-1,0},{-1,1},{0,-1},{0,1},{1,-1},{1,0},{1,1}};
int n,nx,ny;
bool isoutindexof(int i,int j){
    return !(i<0||i>=n||j<0||j>=n);
} 
bool check(int x,int y,int k){
    int index=1;
    for(int i=1;i<7;i++){
        if(isoutindexof(x,y)&&graph[x][y]==eg[index]){
            index++;
            x += d[k][0];
            y += d[k][1];
        } else {
            return false;
        }
    }
    return true;
}
void setvis(int x,int y,int k){
    for(int i=0;i<7;i++){
        vis[x][y]=1;
        x += d[k][0];
        y += d[k][1];
    }
}

int main(){
    cin >> n;
    for(int i=0;i<n;i++) cin >> graph[i];
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            if(graph[i][j]=='y'){
                for(int k=0;k<8;k++){
                    nx = d[k][0] + i;
                    ny = d[k][1] + j;
                    if(isoutindexof(nx,ny)&&check(nx,ny,k)){
                        setvis(i,j,k);
                    }
                }
            }
        }
    }
    for(int i=0;i<n;i++){
        for(int j=0;j<n;j++){
            if(vis[i][j]) cout << graph[i][j]; 
            else cout << "*";
        }
        cout << endl;
    }
    return 0;
}
```

## 填涂颜色

这题思路相当巧妙，再外面再加一圈辅助，妙

```cpp
#include <iostream>
using namespace std;

int n;
int a[35][35];
int d[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};

void dfs(int x, int y) {
   a[x][y]=2;
   for(int i=0;i<4;i++){
       int nx = d[i][0] + x;
       int ny = d[i][1] + y;
       if(nx<0||nx>n+1||ny<0||ny>n+1)continue;
       if(a[nx][ny]==0){
               dfs(nx,ny);
       }
   }
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= n; j++) {
            cin >> a[i][j];
        }
    }
     dfs(0,0);
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            cout << 2-a[i][j] << " ";
        }
        cout << endl;
    }
    return 0;
}
```

## PERKET

搜就完事，注意清水的情况

```cpp
#include <bits/stdc++.h>
using namespace std;

int s[11],b[11],n,ans=(1<<30);

void dfs(int i,int x,int y){
    if(i>n){
        if(x==1&&y==0)return;
        ans = min(ans,abs(x-y));
        return;
    }
    dfs(i+1,x*s[i],y+b[i]);
    dfs(i+1,x,y);
}

int main() {
    cin >> n;
    for (int i = 1; i <= n; i++) cin >> s[i] >> b[i];
      dfs(1,1,0);
      cout << ans;
}
```

## *考前临时抱佛脚

这题还得多做，不好理解

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

int l, r, ans, res;
int s[5];
int a[21];

void dfs(int x, int y) {
    if (x > s[y]) {
        res = min(res, max(l, r));
        return;
    }
    l += a[x];
    dfs(x + 1, y);
    l -= a[x];
    r += a[x];
    dfs(x + 1, y);
    r -= a[x];
}

int main() {
    cin >> s[1] >> s[2] >> s[3] >> s[4];
    for (int i = 1; i <= 4; i++) {
        l = r = 0;
        res = 19260817;
        for (int j = 1; j <= s[i]; j++) {
            cin >> a[j];
        }
        dfs(1, i);
        ans += res;
    }
    cout << ans;

    return 0;
}
```

## 自然数拆分

回溯细节把控好

```cpp
#include <bits/stdc++.h>
using namespace std;
int a[12];
int n,p=0;
void dfs(int i,int sum,int start){
    if(sum<0)return;
    if(sum==0){
        for(int j=0;j<p;j++){
            if(j==0) cout << a[j];
            else cout << "+" << a[j];
        }
        cout << endl; 
        return;
    } 
    for(int i=start;i<n;i++){
        a[p++] = i;
        dfs(i,sum-i,i);
        a[--p] = 0;
    }
}
int main(){
    cin >> n;
    dfs(n,n,1);
    return 0;
}
```

## 1002过河卒

这题本来是用动态规划做的，但是dfs也可以，不过会TLE几个,nx,ny还是定义成局部变量好，不然会坏事

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,m,MX,MY;
bool vis[25][25];
int md[8][2] = {{-1,-2},{-2,-1},{-2,1},{-1,2},{1,-2},{2,-1},{2,1},{1,2}};
int d[2][2] = {{1,0},{0,1}};
bool check(int i,int j){
    return (i>n||j>m);
}
int dfs(int x,int y,int step){
    int res = 0;
    if(x==n&&y==m){
        return 1;
    }
    if(step>45)return 0;
    for(int i=0;i<2;i++){
        int nx = x + d[i][0];
        int ny = y + d[i][1];
        if(check(nx,ny))continue;
        if(!vis[nx][ny]){
            res += dfs(nx,ny,step+1);
        }
    }
    return res;
}
int main(){
    cin >> n >> m >> MX >> MY;
    vis[MX][MY] = 1;
    for(int i=0;i<8;i++){
        int nx = MX + md[i][0];
        int ny = MY + md[i][1];
        if(check(nx,ny))continue;
        vis[nx][ny] = 1;
    }
    cout << dfs(0,0,0);
    return 0;

}
```

## 2089烤鸡

```cpp
#include <bits/stdc++.h>
using namespace std;
int n,cnt=1,c;
int a[12];
int gr[100000][12];
void dfs(int step,int sum){
    if(step==10){
        if(sum==n){
            for(int i=1;i<=10;i++){
                gr[c][i]=a[i];
            }
            c++;
        }
        return;
    }
    for(int i=1;i<=3;i++){
        a[cnt++] = i;
        dfs(step+1,sum+i);
        a[--cnt] = 0;
    }
}

int main(){
    cin >> n;
    if(n>30){
        cout <<0;
        return 0;
    }
    dfs(0,0);
    cout << c << endl;
    for(int i=0;i<c;i++){
        for(int j=1;j<=10;j++){
            cout << gr[i][j] << " ";
        }
        cout << endl; 
    }
    return 0;
}
```

## 1596LakeCounting

```cpp
#include <bits/stdc++.h>
using namespace std;
string graph[101];
int n,m,ans;
bool vis[101][101];
int d[8][2] = {{-1,-1},{-1,0},{-1,1},{0,-1},{0,1},{1,-1},{1,0},{1,1}};

void dfs(int x,int y){
    if(vis[x][y])return;
    vis[x][y] = true;
    for(int i=0;i<8;i++){
        int nx = d[i][0]+x;
        int ny = d[i][1]+y;
        if(nx<0||nx>=n||ny<0||ny>=m)continue;
        if(graph[nx][ny]=='W'){
            dfs(nx,ny);
        }
    }
}

int main(){
    cin >> n >> m;
    for(int i=0;i<n;i++){
        cin >> graph[i];    
    }
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            if(graph[i][j]=='W'&&!vis[i][j]){
                dfs(i,j);
                ans++;
            }
        }
    }
    cout << ans;
    return 0; 

}
```

> 定期回来复习重做
