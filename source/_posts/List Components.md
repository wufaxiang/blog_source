---
title: List Components
date: 2015-01-03 19:53:12
tags: 
  - PAT
  - 数据结构
  - List
category: PAT
---

粘题目：

&emsp;&emsp;For a given undirected graph with N vertices and E edges, please list all the connected components by both DFS and BFS. Assume that all the vertices are numbered from 0 to N-1. While searching, assume that we always start from the vertex with the smallest index, and visit its adjacent vertices in ascending order of their indices.
<!-- more -->
** Input Specification: **

&emsp;&emsp;Each input file contains one test case. For each case, the first line gives two integers N (0<N<=10) and E, which are the number of vertices and the number of edges, respectively. Then E lines follow, each described an edge by giving the two ends. All the numbers in a line are separated by a space.

** Output Specification: **

&emsp;&emsp;For each test case, print in each line a connected component in the format "{ v1 v2 ... vk }". First print the result obtained by DFS, then by BFS.

Sample Input:

    
    
    8 6
    0 7
    0 1
    2 0
    4 1
    2 4
    3 5
    

Sample Output:

    
    
    { 0 1 4 2 7 }
    { 3 5 }
    { 6 }
    { 0 1 2 7 4 }
    { 3 5 }
    { 6 }

  

说下题的输入和输出

输入：

第一行顶点的个数和边的个数

·  后面是每条边所对应的2个顶点

输出：

就是输出DFS和BFS序列，注意点格式就好

  

今天的这道题就是求图的DFS和BFS遍历序列，存储图时我使用了图的邻接矩阵存储，当然也可以使用邻接表，请自己尝试，本题比较简单，直接上代码：

```C++
    #include<iostream>
    #include<queue>
    #define MaxSize 100
    using namespace std;
    typedef int ElemType;
    
    typedef struct
    {
    	ElemType vex[MaxSize];
    	bool flag[MaxSize][MaxSize];
    	int vexnum;
    	int edg;
    }Graph;
    bool visited[MaxSize];
    void DFS(Graph G, int v)
    {
    	cout << v<<" ";
    	visited[v] = true;
    	for (int i = 0; i < G.vexnum; i++)
    	{
    		if (G.flag[v][i] == true && (!visited[i]))
    			DFS(G, i);
    	}
    }
    
    void DFSTraverse(Graph G)
    {
    	for (int v = 0; v < G.vexnum; v++)
    		visited[v] = false;
    	for (int v = 0; v < G.vexnum; v++)
    	{
    		if (!visited[v])
    		{
    			cout << "{ ";
    			DFS(G,v);
    			cout << "}" << endl;
    		}
    	}
    }
    
    void BFS(Graph G,int v)
    {
    	cout << v << " ";
    	visited[v] = true;
    	queue<ElemType> q;
    	q.push(v);
    	while (!q.empty())
    	{
    		v = q.front();
    		q.pop();
    		for (int w = 0; w < G.vexnum; w++)
    		{
    			if (!visited[w] && G.flag[v][w])
    			{
    				cout << w << " ";
    				visited[w] = true;
    				q.push(w);
    			}
    		}
    	}
    }
    
    void BFSTraverse(Graph G)
    {
    	for (int v = 0; v < G.vexnum; v++)
    		visited[v] = false;
    	for (int v = 0; v < G.vexnum; v++)
    	{
    		if (!visited[v])
    		{
    			cout << "{ ";
    			BFS(G, v);
    			cout << "}" << endl;
    		}
    	}
    }
    int main()
    {
    	int N, E;
    	cin >> N >> E;
    	Graph G;
    	G.vexnum = N;
    	G.edg = E;
    	for (int i = 0; i < N; i++)
    		for (int j = 0; j < N; j++)
    			G.flag[i][j] = false;                       //初始化
    	//构造一个二维数组存储是否有边
    	for (int i = 0; i < E; i++)
    	{
    		ElemType a, b;
    		cin >> a >> b;
    		G.flag[a][b] = true;
    		G.flag[b][a] = true;
    	}   
    	for (int i = 0; i < N; i++)
    	{
    		G.vex[i] = i;
    	}
    	DFSTraverse(G);
    	BFSTraverse(G);
    }

```

