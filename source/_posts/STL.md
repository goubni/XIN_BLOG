---
title: C++的STL容器(比赛专用)
excerpt: 复习，上手更快的C++
quicklink: true
tags: STL
categories: C++学习
date: 2023-10-14 23:21:00

---



# STL



## vector

 vector为可变长数组（动态数组），定义的vector数组可以随时添加数值和删除元素

- 头文件
  
  - > #include<vector>

- 初始化
  
  - > vector<int> c //定义了一个数据类型为int的数组
    > 
    > vector<double> c(n) //定义了一个数据类型为double的，长度为n，且元素全为0的数组
    > 
    > vector<int> c(n,1) //定义了一个数据类型为int的，长度为n，前元素全为1的数组

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
  
  - ```cpp
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



## stack

 栈为数据结构的一种，是一个先进后出，后进先出的容器, 可类比java中的Stack，和python中的list

- 头文件
  
  - > #include<stack>

- 初始化
  
  - > stack<int> s;
    > 
    > stack<string> s;
    > 
    > stack<node> s;

- 函数运用
  
  - | 代码        | 含义           |
    |:---------:|:------------:|
    | s.pop()   | 移出栈顶元素O(1)   |
    | s.push(6) | 将6压入栈内O(1)   |
    | s.top()   | 取出栈顶元素O(1)   |
    | s.empty() | 判断栈是否为空O(1)  |
    | s.size()  | 返回栈中元素个数O(1) |

- 代码示例
  
  - ```cpp
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



## priority_queue

优先队列嘛

- 头文件
  
  - > #include<queue> 没错，只要引入一个queue就可以了

- 初始化
  
  - > priority_queue<int> pq; 创建了一个名为pq优先队列

- 函数运用
  
  - 和普通的队列一样，不过它有排序的功能,插入和弹出都得花O(logn)
  
  - 设置优先级：
    
    - > priority_queue<int> pq; //默认是大根堆，弹出的是最大的值
      > 
      > priority_queue<int,greater<int>>pq; //小根堆，弹出的是最小的值

- 示例
  
  - ```cpp
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
  
  - > #include<map>
    > 
    > #include<unordered_map>

- 初始化
  
  - > map<int,string> mp;
    > 
    > map<int,double> mp;
    > 
    > map<int,node> mp;

- 函数运用

- 


