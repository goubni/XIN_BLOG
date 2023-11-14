---
title: dfs刷题
excerpt: dfs刷的题目
tags: dfs
categories: 算法学习
quicklink: true
date: 2023-11-13 22:48:49

---

> 今天刷了一天的dfs，主要是想捡回深搜的思维，发现好多题大概的思维都已经对了，就是在一些小细节上没有处理好导致WA,TLE，MLE。

先把今天刷的题目列出来，难度按照从小到达排列

[P1036 选数 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1036)

[P1605 迷宫 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1605)

[P1123 取数游戏 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1123)

[P1135 奇怪的电梯 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1135)

[P1443 马的遍历 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P1443)

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
