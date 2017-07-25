---
title: mooc -- 06-4 How Long Does It Take
date: 2015-01-18 13:02:12
tags: 
  - PAT
  - 数据结构
category: PAT
---

题目：

&emsp;&emsp;Given the relations of all the activities of a project, you are supposed to find the earliest completion time of the project.

** Input Specification: **

&emsp;&emsp;Each input file contains one test case. Each case starts with a line containing two positive integers N (<=100), the number of activity check points (hence it is assumed that the check points are numbered from 0 to N-1), and M, the number of activities. Then M lines follow, each gives the description of an activity. For the i-th activity, three non-negative numbers are given: S[i], E[i], and L[i], where S[i] is the index of the starting check point, E[i] of the ending check point, and L[i] the lasting time of the activity. The numbers in a line are separated by a space.
<!-- more -->
** Output Specification: **

&emsp;&emsp;For each test case, if the scheduling is possible, print in a line its earliest completion time; or simply output "Impossible".

** Sample Input 1: **
    
    
    9 12
    0 1 6
    0 2 4
    0 3 5
    1 4 1
    2 4 1
    3 5 2
    5 4 0
    4 6 9
    4 7 7
    5 7 4
    6 8 2
    7 8 4
    

** Sample Output 1: **
    
    
    18
    

** Sample Input 2: **
    
    
    4 5
    0 1 1
    0 2 2
    2 1 3
    1 3 4
    3 2 5
    

** Sample Output 2: **
    
    
    Impossible
    

这道题就是一道拓扑排序的题求最短完成时间

注意：

输出最大值，可以用标准库的函数：max_element，返回值是地址，所以需要*把内容取出来。如数组a[n]，最大值为：*max_element(earlist, earlist+n)。当然不是求数组的最大值，而是求vector的最大值，只需要传相应的迭代器就可以。

  

代码中都有说明，直接上：

```C++    
    
    #include<iostream>
    #include <algorithm>
    #include <vector>
    #include <queue>
    using namespace std;
     
    struct node       //定义结构结点 
    {
    	int S;
    	int E;
    	int L;
    	node(int a,int b,int c):S(a),E(b),L(c) {}
    } ;
     
     //cmp函数（用于sort排序函数的子函数）
    bool cmp(const node &a, const node &b)
    {
    	return a.E < b.E; 
    } 
      
    int main()
    {
    	//最早完成时间、入度数组的动态申请及初始化 
    	int N,M;
    	cin>>N>>M;
      	int *earlist = new int[N];
    	int *Indegree = new int[N];
    	
    	for(int i=0;i<N;i++)
    	{
    		earlist[i] = 0;
    		Indegree[i] = 0;
    	}
    	
    	vector<node> vec;        //定义一个容器存放入度为0 的结点 
    	//s，e输入并存储 
    	for(int i=0;i<M;i++)
    	{
    		int s,e,l;
    		cin>>s>>e>>l;
    		Indegree[e] ++ ;
    		vec.push_back(node(s,e,l)); 
    	}
    	//sort函数用于排序，把终点相同的活动集中在一起 
    	sort(vec.begin(), vec.end(), cmp);
       
    	queue<int> q;
    	
    	for(int v=0;v<N;v++)
    	{
    		if(Indegree[v] == 0)
    			q.push(v);
    	}    
    	//拓扑排序 
    	int cnt = 0;
    	
    	while( q.size() != 0)
        {
            int v = q.front();
            q.pop();
            cnt++;
            for ( int i=0; i<M; i++ )
            {
                if ( vec[i].S == v)
                {
                    int w = vec[i].E;
                    earlist[w] = max(earlist[w], earlist[v] + vec[i].L);
                    if ( --Indegree[w] == 0)
                        q.push(w);
                }
            }
    
        }
    	//判断是否存在回路 
    	if(cnt != N)
    		cout<<"Impossible"<<endl;
    	else
    		cout<<*max_element(earlist, earlist+N)<<endl;
    	return 0;
    }

  
```

