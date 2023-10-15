---
title: C++的STL容器(比赛专用)
excerpt: 复习，上手更快的C++
quicklink: true
tags: STL
categories: C++学习
date: 2023-10-14 23:21:00

---



## vector

 vector为可变长数组（动态数组），定义的vector数组可以随时添加数值和删除元素

- 头文件
  
  > #include<vector>

- 初始化
  
  > vector<int> c //定义了一个数据类型为int的数组
  > 
  > vector<double> c(n) //定义了一个数据类型为double的,长为n
  > 
  > vector<int> c(n,1) //定义了一个数据类型为int的，长为n，全是1

- 函数运用
  
  | 代码             | 含义                       |
  |:--------------:|:------------------------:|
  | c.front()      | 取出这个数组的第一个元素 O(1)        |
  | c.pop_back()   | 弹出这个数组的最后一个元素,不会返回值。O(1) |
  | c.push_back(6) | 在尾部添加一个数据，这个数据是6。O(1)    |
  | c.size()       | 返回数组的大小O（1）              |
  | c.begin()      | 返回首元素的地址(迭代器)            |
  | c.end()        | 返回最后一个元素的地址(迭代器)         |
  | c.empty()      | 判断是否为空O（1）               |

- 代码示例
  
  ```cpp
  #include<iostream>
  #include<algorithm>
  #include<vector>
  using namespace std;
  int main() {
        vector<int> a;
        a.push_back(3);
        a.push_back(2);
        a.push_back(1);
        int first = a.front();
        sort(a.begin(),a.end());
        for(auto x:a){
            cout<<x;
        }
        cout << " 这个数组长度为" << a.size() << ",里面的元素排序后是 1 2 3" << endl; 
        return 0;
  } 
  ```

<br>

<br>

## stack

栈为数据结构的一种，是一个先进后出，后进先出的容器, 可类比java中的Stack，和python中的list

- 头文件
  
  > #include<stack>

- 初始化
  
  > stack<int> s;
  > 
  > stack<string> s;
  > 
  > stack<double> s;

- 函数运用
  
  | 代码        | 含义           |
  |:---------:|:------------:|
  | s.pop()   | 移出栈顶元素O(1)   |
  | s.push(6) | 将6压入栈内O(1)   |
  | s.top()   | 取出栈顶元素O(1)   |
  | s.empty() | 判断栈是否为空O(1)  |
  | s.size()  | 返回栈中元素个数O(1) |

- 代码示例
  
  ```cpp
  #include<iostream>
  #include <stack>
  using namespace std;
  
  int main() {
      //创建了一个栈 
      stack<int> st;
      st.push(1);
      st.push(2);
      st.push(3);
      cout << "第一次弹出元素是" << st.top() << endl;
      st.pop(); 
      cout << "第一次弹出元素是" << st.top() << endl;
      st.pop();
      cout << "第一次弹出元素是" << st.top() << endl;
      return 0;
   } 
  ```
  
  - 第一次弹出元素是3
    第一次弹出元素是2
    第一次弹出元素是1

<br>

<br>

## priority_queue

deque和queue只是没有了排序功能，一个是双端队列，一个是队列

优先队列嘛

- 头文件
  
  > #include <queue> //没错，只要引入一个queue就可以了

- 初始化
  
  > priority_queue<int> pq; //创建了一个名为pq优先队列

- 函数运用
  
  1. 和普通的队列一样，不过它有排序的功能,插入和弹出都得花O(logn)
  
  2. 设置优先级：
     
     > priority_queue<int> pq; //默认是大根堆，弹出的是最大的值
     > 
     > priority_queue<int,greater>pq; //小根堆，弹出的是最小的值

- 示例
  
  ```cpp
  #include <iostream>
  #include <queue>
  using namespace std;
  
  int main() {
      priority_queue<int> pq;
      pq.push(8);
      pq.push(4);
      pq.push(12);
      cout << "第一次弹出元素是" << pq.top() << endl;
      pq.pop(); 
      cout << "第一次弹出元素是" << pq.top() << endl;
      pq.pop();
      cout << "第一次弹出元素是" << pq.top() << endl;
      return 0;
   } 
  ```
  
  - 输出结果：第一次弹出元素是12
    第一次弹出元素是8
    第一次弹出元素是4

<br>

<br>

## map

map特性：map会按照键的顺序从小到大自动排序

这里map是基于红黑树实现的，相当强大，有排序功能，unordereed_map它是基于哈希表实现的，各有优缺点。这里讲map，得根据实际情况选择map还是unordered_map

| map                   | unordered_map                                                     |
|:---------------------:|:-----------------------------------------------------------------:|
| 基于红黑树实现               | 基于哈希表实现                                                           |
| 插入是O(logn),查询是O(logn) | 插入是O(logn),查询是O(1)                                                |
| map可以支持所有类型的键值对       | unordered_map的使用比较局限，它的key只能是int、double等基本类型以及string，而不能是自己定义的结构体 |

set和unordered_set也是上面这种情况

使用建议：不涉及到排序就无脑unordered_map、unordered_set。否则map、set

- 头文件
  
  > #include<map>
  > 
  > #include<unordered_map>

- 初始化
  
  > map<int,string> mp;
  > 
  > map<int,double> mp;
  > 
  > map<int,node> mp;

- 函数运用
  
  | 代码                   | 含义                                                                |
  |:--------------------:|:-----------------------------------------------------------------:|
  | mp.find(key)         | 寻找map中的键O(logn),找到之后返回一个迭代器，当数据存在时，返回数据所在位置的迭代器，数据不存在时，返回mp.end() |
  | mp.erase(it)         | 删除这个迭代器对应的键值对O(1)                                                 |
  | mp.erase(key)        | 删除这个键与它映射的值O(logn)                                                |
  | mp.size()            | 返回映射的对数O(1)                                                       |
  | mp.clear()           | 清除所有键值对 O(logn)                                                   |
  | mp.insert()          | 插入元素，构造键值对O(logn)                                                 |
  | mp.empty()           | 判断是否为空O(1)                                                        |
  | mp.begin(),mp.rend() | 返回指向map的第一个元素的迭代器O(1)                                             |
  | mp.end(),mp.rbegin() | 返回指向map的最后一个元素迭代器O(1)                                             |
  | mp.count(key)        | 查找元素是否存在，存在返回1，不存在返回0 O(logn)                                     |
  | mp.lower_bound()     | (二分查找)返回一个迭代器，指向键>=key的位置                                         |
  | mp.upper_bound()     | 返回一个迭代器，指向键>key的位置                                                |

- 代码示例
  
  ```cpp
  #include<algorithm>
  #include <iostream>
  #include <map> 
  using namespace std;
  
  int main() {
      map<string,int> mp;
  
      //四种增加键值对的方式 
      mp["张三"] = 18;
      mp.insert({"李四",18});
      mp.insert(make_pair("赵六",18));
      mp.insert(pair<string,int>("钱七",18));
  
      //遍历（推荐） 
      cout << mp.find("张三")->first << endl;
      for(auto x:mp){
          cout << x.first << " " << x.second << endl;
      }
  
      //正序遍历，先获得begin()的地址 
      auto iter = mp.begin();
      while(iter != mp.end()){
          cout << iter->first << iter->second << endl;
          iter++;
      }
  
      //逆序遍历，先获得rbegin()的地址
      auto it = mp.rbegin();
      while(it != mp.rend()){
          cout << it->first << it->second << endl;
          it++;
      }
  
      map<int, string> mp2;
      mp2[1] = "星期一";
      mp2[2] = "星期二";
      mp2[3] = "星期三";
      mp2[4] = "星期四";
      cout << mp2.find(3)->second << endl;  //输出的是星期三
      cout << mp2.find(4)->second << endl;  //输出的是星期四
      cout << mp2.lower_bound(3)->second << endl;  //输出的是星期三 
  
      cout << mp2.size() << endl;  //现在是4 
      //将第一个元素的键值对删掉 
      mp2.erase(mp2.begin());
      cout << mp2.size() << endl;  //现在是3 
      return 0;
   } 
  ```

> 注意：当要查找一个map中的元素时，有三种方法：
> 
> 1. mp.find();
> 
> 2. mp.count();
> 
> 3. mp[key]  //这种做法不推荐，如果第一次没有找到元素，那将会自动为这个键映射到一个值0上。浪费空间

还有一种映射,multimap键可以重复，即一个键对应多个值

<br>

## set

set具有去重功效,函数基本和上面的map很像，时间复杂度也是一样，而且默认set是从小到大排序的，如果要不是排序的，可以用unordered_set。

可以类比python，java中的类

| c++                | python     | java      |
|:------------------:|:----------:|:---------:|
| map                | SortedDict | TreeMap   |
| unordered_map      | dict       | HashMap   |
| set                | SortedSet  | TreeSet   |
| unorded_set        | set        | HashSet   |
| multiset           | SortedList | 俺不知道      |
| unordered_multiset | list       | ArrayList |

```cpp
#include <iostream>
#include <set>
using namespace std;

int main() {
    set<int> s;
    s.insert(1);
    s.insert(1);
    s.insert(2);
    //迭代器遍历 
    auto it = s.begin();
    while (it != s.end()){
        cout << *it << endl;
        it++;
    }

    //智能指针
    for(auto x: s){
        cout << x << endl;
    } 
    return 0;
 } 
```

输出的是 1 2 1 2

- 比较规则
  
  - > set<int> s; //从小到大排序
    > 
    > set<int, greater<int>> s; // 从大到小排序
  
  - 自定义建立比较规则（通用）
    
    > ```cpp
    > #include<algorithm>
    > #include <iostream>
    > #include <set>
    > using namespace std;
    > 
    > struct Point{
    >     int x;
    >     int y;
    >     bool operator() (const Point &p1, const Point &p2) {
    >         if (p1.x == p2.x) return p1.y < p2.y;
    >         return p1.x < p2.x;
    >     }
    > };
    > 
    > int main() {
    >     set<Point, Point> s;//第二个Point代表自定义函数比较规则
    >     Point p1 = {3,4};
    >     Point p2 = {3,5};
    >     Point p3 = {3,7};
    >     Point p4 = {1,9};
    >     s.insert(p1);
    >     s.insert(p2);
    >     s.insert(p3);
    >     s.insert(p4);
    >     for(auto a:s){
    >         cout << a.x << " " << a.y << endl;
    >     }
    >     return 0;
    > }
    > ```

        

## cin,cout解锁

> 解锁 `cin` 和 `cout` 的常见方法。基本思想是通过取消 `cin` 和 `cout` 与 C 标准库的关联，从而提高其读取和写入的速度。
> 
> 默认情况下，`cin` 与 `cout` 是与 C 标准输入输出流(stdin, stdout)同步的，并且它们的性能受到了一些缓冲区同步的限制，这会导致在大量输入输出时速度较慢。
> 
> 而通过使用 `ios::sync_with_stdio(false)` 可以关闭 `cin` 和 `cout` 与 C 标准库的同步，使其独立运行，不再受到缓冲区同步的限制。同时，通过 `cin.tie(0)` 和 `cout.tie(0)` 可以取消 `cin` 和 `cout` 的绑定，进一步提高它们的速度。
> 
> 这样做的目的是为了在处理大量输入输出的情况下，加快程序运行的速度，避免超时。然而，需要注意的是，在使用解锁的 `cin` 和 `cout` 时，不能与其他输入输出函数（如 `scanf`、`getchar`、`printf`、`cin.getline()`）混用，否则可能会导致错误。

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    //解锁cin，cout 
    ios::sync_with_stdio(false);
    //取消绑定 
    cin.tie(0);cout.tie(0);
    string s;
    getline(cin,s);
    cout << s;
    return 0;
 } 
```

<br>

> python刷题转c++真累，两边都要兼顾，蓝桥用python打，后面的比赛又得用c++😵😵
