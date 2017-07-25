---
title: Saving James Bond
date: 2015-01-04 23:58:12
tags: 
  - 数据结构
  - 图
category: PAT
---

题目：

&emsp;&emsp;This time let us consider the situation in the movie "Live and Let Die" in which James Bond, the world's most famous spy, was captured by a group of drug dealers. He was sent to a small piece of land at the center of a lake filled with crocodiles. There he performed the most daring action to escape -- he jumped onto the head of the nearest crocodile! Before the animal realized what was happening, James jumped again onto the next big head... Finally he reached the bank before the last crocodile could bite him (actually the stunt man was caught by the big mouth and barely escaped with his extra thick boot).

&emsp;&emsp;Assume that the lake is a 100 by 100 square one. Assume that the center of the lake is at (0,0) and the northeast corner at (50,50). The central island is a disk centered at (0,0) with the diameter of 15. A number of crocodiles are in the lake at various positions. Given the coordinates of each crocodile and the distance that James could jump, you must tell him whether or not he can escape.
<!-- more -->
** Input Specification: **

&emsp;&emsp;Each input file contains one test case. Each case starts with a line containing two positive integers N (<=100), the number of crocodiles, and D, the maximum distance that James could jump. Then N lines follow, each containing the (x, y) location of a crocodile. Note that no two crocodiles are staying at the same position.

** Output Specification: **

&emsp;&emsp;For each test case, print in a line "Yes" if James can escape, or "No" if not.

Sample Input 1:

    
    
    14 20
    25 -15
    -25 28
    8 49
    29 15
    -35 -2
    5 28
    27 -29
    -8 -28
    -20 -35
    -25 -20
    -13 29
    -30 15
    -35 40
    12 12
    

Sample Output 1:

    
    
    Yes
    

Sample Input 2:

    
    
    4 13
    -12 12
    12 12
    -12 -12
    12 -12
    

Sample Output 2:

    
    
    No

  

解释一下题目：

这是一个有趣的题目，是一个007逃生的问题，也就是图的遍历啦

输入：鳄鱼的数量+007的跳跃半径+鳄鱼的坐标。  

输出：Yes能逃出，No不能逃出。  

  

这道题是不需要构造图的，我讲一下我的思路吧，也就是陈姥姥讲的：

1.输入鳄鱼数目和007的跳跃距离

2.最开始要判断一下就是可不可以直接到达岸边，如果可以就输入yes，结束程序

3.输入鳄鱼坐标，我是采用了pair容器存储，再将所有的鳄鱼坐标放到了vector容器中存储

4.同时需要定义一个vector 用来存放该结点是否被访问过

5.要开始进行遍历了，第一跳不一样的，要注意，因为还有个小岛的半径。

开始分析：我的方法是，先找出所有007第一跳能达到的鳄鱼结点，将他们的序列号存放到一个我定义为firstJump的vector容器中

6.就对firstJump中的第一跳鳄鱼结点进行遍历

7.遍历时，先判断从此个结点是否可以到达岸边，如果可以，直接输出yes，结束程序

不能达到，在对所有结点进行分析，找出下一个可以到达的结点，再以这个结点，进行遍历，也就是递归

8.如果都不能到达，最后就输出“No”;

  

源码：

```C++
    
    #include<iostream>
    #include<vector>
    #include<cmath>
    using namespace std;
    
    #define BORDER 50
    #define ISLAND 7.5
    typedef pair<int, int> crocodiles;		 //鳄鱼的坐标
    int cro_num, jumpsize;          //鳄鱼的数目 和 跳跃半径
    vector<bool> visited;       //判断是否遍历过
    vector<crocodiles> vec;      //存鳄鱼结点
    double Distance(crocodiles a, crocodiles b)
    {
    	return sqrt((b.second - a.second)*(b.second - a.second) + (b.first - a.first)*(b.first - a.first));
    }
    
    
    
    void BFS(int i)
    {
    	visited[i] = true;
    	if ((50 - abs(vec[i].first)) <= jumpsize || (50 - abs(vec[i].second)) <= jumpsize)
    	{
    		cout << "Yes";
    		exit(0);
    	}
    	for (int j = 0; j < cro_num; j++)
    	{
    		if (!visited[j] && Distance(vec[i], vec[j]) <= jumpsize)
    		{
    			BFS(j);
    		}
    	}
    }
    
    int main()
    {
    	cin >> cro_num >> jumpsize;
    	for (int i = 0; i < cro_num; i++)
    	{
    		int x, y;
    		cin >> x >> y;                      //输入坐标
    		vec.push_back(make_pair(x, y));             //存到vec容器中
    		visited.push_back(false);
    	}
    	//如果第一跳可以直接抵达岸边
    	if (jumpsize > BORDER - ISLAND)
    	{
    		cout << "Yes";
    		return 0;
    	}
    	//第一跳能到达的鳄鱼结点
    	crocodiles O = make_pair(0, 0);//定义原点
    	vector<int> firstJump;           //用来存放第一跳可以抵达的鳄鱼结点的
    	for (int i = 0; i < cro_num; i++)
    	{
    		if (Distance(vec[i], O) <= (jumpsize + ISLAND))
    		{
    			firstJump.push_back(i);                   //存入
    		}
    	}
    	if (firstJump.empty())           //如果第一跳不能到任意一条鳄鱼
    	{
    		cout << "No";
    		return 0;
    	}
    	for (int i = 0; i < firstJump.size(); i++)         //遍历
    	{
    		BFS(firstJump[i]);
    	}
    	cout << "No";
    	return 0;
    }

```
  

