---
title: Saving James Bond - Hard Version
date: 2015-01-14 17:36:12
tags: 
  - 数据结构
  - 图
category: PAT
---

题目：

&emsp;&emsp;This time let us consider the situation in the movie "Live and Let Die" in which James Bond, the world's most famous spy, was captured by a group of drug dealers. He was sent to a small piece of land at the center of a lake filled with crocodiles. There he performed the most daring action to escape -- he jumped onto the head of the nearest crocodile! Before the animal realized what was happening, James jumped again onto the next big head... Finally he reached the bank before the last crocodile could bite him (actually the stunt man was caught by the big mouth and barely escaped with his extra thick boot).

Assume that the lake is a 100 by 100 square one. Assume that the center of the lake is at (0,0) and the northeast corner at (50,50). The central island is a disk centered at (0,0) with the diameter of 15. A number of crocodiles are in the lake at various positions. Given the coordinates of each crocodile and the distance that James could jump, you must tell him a shortest path to reach one of the banks. The length of a path is the number of jumps that James has to make.
<!-- more -->
** Input Specification: **

&emsp;&emsp;Each input file contains one test case. Each case starts with a line containing two positive integers N (<=100), the number of crocodiles, and D, the maximum distance that James could jump. Then N lines follow, each containing the (x, y) location of a crocodile. Note that no two crocodiles are staying at the same position.

** Output Specification: **

&emsp;&emsp;For each test case, if James can escape, output in one line the minimum number of jumps he must make. Then starting from the next line, output the position (x, y) of each crocodile on the path, each pair in one line, from the island to the bank. If it is impossible for James to escape that way, simply give him 0 as the number of jumps. If there are many shortest paths, just output the one with the minimum first jump, which is guaranteed to be unique.

** Sample Input 1: **
    
    
    17 15
    10 -21
    10 21
    -40 10
    30 -50
    20 40
    35 10
    0 -10
    -25 22
    40 -40
    -30 30
    -10 22
    0 11
    25 21
    25 10
    10 10
    10 35
    -30 10
    

** Sample Output 1: **
    
    
    4
    0 11
    10 21
    10 35
    

** Sample Input 2: **
    
    
    4 13
    -12 12
    12 12
    -12 -12
    12 -12
    

** Sample Output 2: **
    
    
    0
    

======================================================

&emsp;&emsp;这道题是上次的一个升级版，上次的只需要求出007是否能逃出来来，而这次则需要求出如果能逃出来，那么最短的一个路径是哪一条，本来打算在原本那道的基础上直接修改的，可是改了好久都是没结果，表示十分难过

&emsp;&emsp;后来发现了一位大神的做法，重新建图，用邻接表进行存储，这次又用到了优先队列，这个队列有解决了求最短的问题，由于源码中解释的十分详细，所以直接看源码吧

  

  

源码：

    
```C++   
    #include <iostream>
    #include <algorithm>
    #include <vector>
    #include <cmath>
    #include <queue>
    #include <stack>
    
    using namespace std;
    
    /*
      * 宏定义的声明
      * FIRSTSTEP 小岛的半径，固定为7.5
      * BORDER 边界的大小，固定为50
      * MAXNUM 无穷大
      */
    
    #define FIRSTSTEP 7.5
    #define BORDER 50
    #define MAXNUM 100000000
    typedef int ElemType;
    typedef pair<double,double> pos;			    //pos 代表鳄鱼的坐标
    vector<pos> vec;								//vec 存储鳄鱼的坐标
    vector<int> pathVec;							//pathVec 用来存储路径
    struct vexNode									//vexNode 邻接表中的结点
    {
    	ElemType data;
    	vexNode *next;
    	vexNode(ElemType d, vexNode *n = NULL) :data(d), next(n) {}
    	bool friend operator<(const vexNode &a, const vexNode &b)
    	{
    		int V = a.data;
    		int W = b.data;
    		double dV = vec[V].first * vec[V].first + vec[V].second * vec[V].second;
    		double dW = vec[W].first * vec[W].first + vec[W].second * vec[W].second;
    		return dV < dW; // 出队先出大的，再出小的
    	}
    };
    vector<vexNode> eVec;						//eVec 邻接表存储图
    
    /*
    	 * 计算两点之间的距离
    	 */
    double Distance(pos p1, pos p2, double dis)
    {
     	double xx = (p1.first - p2.first) * (p1.first - p2.first);
    	double yy = (p1.second - p2.second) * (p1.second- p2.second);
    	if ((p1.first == 0 && p1.second == 0) || (p2.first == 0 && p2.second == 0))
    	{
    		return dis + FIRSTSTEP - sqrt(xx + yy);
    	}
    	else
    	{
    		return dis - sqrt(xx + yy);
    	}
    }
    
    /*
    	* 获得路径
    	*/
    vector<int> getPath(int t, int p[])
    {
    	vector<int> path;
    	for (; t != -1; t = p[t])
    		path.push_back(t);
    	reverse(path.begin(), path.end());
    	return path;
    }
    
    int main()
    {
    	int nNum;
    	double dis;
    	cin >> nNum >> dis;
    	// 考虑特殊情况，能否一步迈出
    	if (dis + FIRSTSTEP >= BORDER)
    	{
    		cout << "1" << endl;
    		return 0;
    	}
    	// 起始点（小岛）也算一个点
    	vec.push_back(pos(0, 0));
    	eVec.push_back(vexNode(0));
    	nNum++;
    	// 用邻接表存储图
    	for (int i = 1; i < nNum; i++)
    	{
    		double a, b;
    		cin >> a >> b;
    		vec.push_back(pos(a, b));
    		eVec.push_back(vexNode(i));
    	}
    	// 开始建图
    	for (int i = 0; i < nNum; i++)
    	{
    		for (int j = 0; j < nNum; j++)
    		{
    			if (i != j)
    			{
    				if (Distance(vec[i], vec[j], dis) >= 0)
    				{
    					// 查一查有没有重复的
    					bool myIFlag = false;
    					vexNode *p = &eVec[i];
    					while (p->next != NULL)
    					{
    						p = p->next;
    						if (p->data == j)
    						{
    							myIFlag = true;
    							break;
    						}
    					}
    					// 如果没有重复的，就插在最后边
    					if (myIFlag == false)
    						p->next = new vexNode(j);
    
    					// 因为是无向图，也就是双向图，所以另一侧也要插
    					bool myJFlag = false;
    					vexNode *q = &eVec[j];
    					while (q->next != NULL)
    					{
    						q = q->next;
    						if (q->data == i)
    						{
    							myJFlag = true;
    							break;
    						}
    					}
    					// 如果没有重复的，就插在最后边
    					if (myJFlag == false)
    						q->next = new vexNode(i);
    				}
    			}
    		}
    	}
    	// 相关数据结构的申请
    	int *dist = new int[nNum];
    	int *path = new int[nNum];
    	priority_queue<vexNode > myQueue;
    
    	// 算法开始
    	// 1. 在相同的最短路里找第一步最小的放入优先级队列中
    
    	vexNode *p = &eVec[0];
    	while (p->next != NULL)
    	{
    		p = p->next;
    		myQueue.push(eVec[p->data]);
    		path[p->data] = 0;
    	}
    	int flag = 1;   // flag用来标记是否是第一次
    	int minDist;    // minDist记录最小的dist值
    
    	// 2. 从岛屿开始，能到达的所有结点做循环
    
    	while (!myQueue.empty())
    	{
    		// 2.1 初始化
    		for (int i = 0; i < nNum; i++)
    		{
    			dist[i] = -1;
    			path[i] = -1;
    		}
    		// 2.2 从队列中弹出一个结点，从这个结点开始，借助另一个队列，进行BFS
    		vexNode vN = myQueue.top();
    		myQueue.pop();
    		path[vN.data] = 0;      // 从myQueue队列中取出的结点，parent一定为岛屿(0,0)
    		queue<int> bfsQueue;    // 进行BFS所需要的队列
    		bfsQueue.push(vN.data);
    		dist[vN.data] = 0;      // 初始的dist值为0
    		while (!bfsQueue.empty())
    		{
    			int W = bfsQueue.front();
    			bfsQueue.pop();
    			// 2.3 判定是不是已经可以上岸了
    			if (fabs(vec[W].first - BORDER) <= dis || fabs(vec[W].first + BORDER) <= dis
    				|| fabs(vec[W].second - BORDER) <= dis || (vec[W].second + BORDER) <= dis)
    			{
    				// 2.3.1 如果是第一次，更新minDist值，并记录路径
    				if (flag&&W != 0)
    				{
    					minDist = dist[W];
    					flag = 0;
    					pathVec = getPath(W, path);
    				}
    				// 2.3.2 如果不是第一次，则比较minDist值与dist值，并更新路径
    				else if (W != 0 && dist[W] <= minDist)
    				{
    					minDist = dist[W];
    					pathVec = getPath(W, path);
    				}
    			}
    			// 2.4 如果没有上岸，则将其邻接结点放入队列中，并更新dist与path的值
    			else
    			{
    				for (int i = 1; i < nNum; i++)
    				{
    					if (Distance(vec[W], vec[i], dis) >= 0 && dist[i] == -1)
    					{
    						bfsQueue.push(i);
    						dist[i] = dist[W] + 1;
    						path[i] = W;
    					}
    				}
    			}
    
    		}
    	}
    
    	// 3. 输出最终结果
    
    	if (pathVec.size() == 0)
    		cout << "0" << endl;
    	else
    	{
    		// 3.1 因为我们把(0,0)也当成结点了，这里不用+1
    		cout << pathVec.size() << endl;
    		for (unsigned int i = 0; i < pathVec.size(); i++){
    			// 3.2 因为我们把(0,0)也当成结点了，但是不能让它输出，所以特殊考虑
    			if (vec[pathVec[i]].first == 0 && vec[pathVec[i]].second == 0);
    
    			else
    				cout << vec[pathVec[i]].first << " " << vec[pathVec[i]].second << endl;
    		}
    		return 0;
    	}
    }

```

