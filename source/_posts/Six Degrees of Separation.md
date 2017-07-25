---
title: Six Degrees of Separation
date: 2015-01-06 23:07:12
tags: 
  - PAT
  - 数据结构
  - 图
category: PAT
---

题目：

&emsp;&emsp;“六度空间”理论又称作“六度分隔（Six Degrees of Separation）”理论。这个理论可以通俗地阐述为：“你和任何一个陌生人之间所间隔的人不会超过六个，也就是说，最多通过五个人你就能够认识任何一个陌生人。”如图6.4所示。

![](http://www.patest.cn/upload/57_mj55i1ensa6.jpg)  
图6.4 六度空间示意图

&emsp;&emsp;“六度空间”理论虽然得到广泛的认同，并且正在得到越来越多的应用。但是数十年来，试图验证这个理论始终是许多社会学家努力追求的目标。然而由于历史的原因，这样的研究具有太大的局限性和困难。随着当代人的联络主要依赖于电话、短信、微信以及因特网上即时通信等工具，能够体现社交网络关系的一手数据已经逐渐使得“六度空间”理论的验证成为可能。

&emsp;&emsp;假如给你一个社交网络图，请你对每个节点计算符合“六度空间”理论的结点占结点总数的百分比。
<!-- more -->
** 输入格式说明： **

&emsp;&emsp;输入第1行给出两个正整数，分别表示社交网络图的结点数N （1<N<=10^4，表示人数）、边数M（<=33*N，表示社交关系数）。随后的M行对应M条边，每行给出一对正整数，分别是该条边直接连通的两个结点的编号（节点从1到N编号）。

** 输出格式说明： **

&emsp;&emsp;对每个结点输出与该结点距离不超过6的结点数占结点总数的百分比，精确到小数点后2位。每个结节点输出一行，格式为“结点编号:（空格）百分比%”。

** 样例输入与输出： **

序号

输入

输出

1

    
    
    10 9
    1 2
    2 3
    3 4
    4 5
    5 6
    6 7
    7 8
    8 9
    9 10
    
    
    
    1: 70.00%
    2: 80.00%
    3: 90.00%
    4: 100.00%
    5: 100.00%
    6: 100.00%
    7: 100.00%
    8: 90.00%
    9: 80.00%
    10: 70.00%
    

2

    
    
    10 8
    1 2
    2 3
    3 4
    4 5
    5 6
    6 7
    7 8
    9 10
    
    
    
    1: 70.00%
    2: 80.00%
    3: 80.00%
    4: 80.00%
    5: 80.00%
    6: 80.00%
    7: 80.00%
    8: 70.00%
    9: 20.00%
    10: 20.00%
    

3

    
    
    11 10
    1 2
    1 3
    1 4
    4 5
    6 5
    6 7
    6 8
    8 9
    8 10
    10 11
    
    
    
    1: 100.00%
    2: 90.91%
    3: 90.91%
    4: 100.00%
    5: 100.00%
    6: 100.00%
    7: 100.00%
    8: 100.00%
    9: 100.00%
    10: 100.00%
    11: 81.82%
    

4

    
    
    2 1
    1 2
    
    
    
    1: 100.00%
    2: 100.00%

  

中文的，就不解释了。

这道题就是一道广度优先遍历的考察

存储方式我使用了邻接表的存储方式，这样更好理解

  

思路：

1.建立一个邻接表存储输入的图

2.对每个结点进行广度优先遍历，并计算层数

3.对层数进行判断，大于等于6就直接break

4.计算比例

  

源码：

```C++
    
    #include<iostream>
    #include<iomanip>
    #include<queue>
    #include<vector>
    using namespace std;
    
    typedef int ElemType;
    struct VexNode{
    	ElemType data;
    	VexNode *next;
    	VexNode(int d, VexNode *n = NULL) :data(d), next(n) {}
    };
    
    vector<VexNode> vec;
    vector<bool> visited;
    
    void output(int i, double x)
    {
    	x *= 100;
    	cout << i << ": " << setprecision(2) << setiosflags(ios::fixed) << x << "%" << endl;
    }
    
    int BFS(int V)
    {
    	visited[V] = true;
    	int icount = 1;
    	int level = 0;
    	int last = V;
    	int tail;
    	queue<int> q;
    	q.push(V);
    	while (q.size() != 0)
    	{
    		int V = q.front();
    		q.pop();
    		VexNode *p = &vec[V];
    		while (p != NULL)
    		{
    			int w = p->data;
    			if (!visited[w])
    			{
    				visited[w] = true;
    				q.push(w);
    				icount++;
    				tail = w;
    			}
    			p = p->next;
    		}
    		if (V == last)
    		{
    			level++;
    			last = tail;
    		}
    		if (level == 6)
    			break;
    	}
    	return icount;
    }
    
    
    void SDS()
    {
    	for (int i = 1; i < vec.size(); i++)
    	{
    		int icount = BFS(i);
    		output(i, (double)icount / (vec.size() - 1));
    		for (int j = 0; j < visited.size(); j++)
    			visited[j] = false;
    	}
    }
    
    int main()
    {
    	int N, E;
    	cin >> N >> E;
    	for (int i = 0; i < N + 1; i++)
    	{
    		vec.push_back(VexNode(i));
    		visited.push_back(false);
    	}
    	// 用“邻接表”形式存储图
    	for (int i = 0; i < E; i++)
    	{
    		int a, b;
    		cin >> a >> b;
    		VexNode *p = &vec[a];
    		while (p->next != NULL)
    		{
    			p = p->next;
    		}
    		p->next = new VexNode(b);
    
    		VexNode *q = &vec[b];
    		while (q->next != NULL)
    		{
    			q = q->next;
    		}
    		q->next = new VexNode(a);
    	}
    	// 调用函数处理问题
    	SDS();
    	return 0;
    }

```

