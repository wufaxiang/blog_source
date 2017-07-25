---
title: 1003. Emergency
date: 2016-03-05 16:26:24
tags: 
  - PAT
  - Dijkstra
  - DFS
category: PAT(A)
---

&emsp;&emsp;As an emergency rescue team leader of a city, you are given a special map of your country. The map shows several scattered cities connected by some roads. Amount of rescue teams in each city and the length of each road between any pair of cities are marked on the map. When there is an emergency call to you from some other city, your job is to lead your men to the place as quickly as possible, and at the mean time, call up as many hands on the way as possible.
<!-- more -->
** Input **

&emsp;&emsp;Each input file contains one test case. For each test case, the first line contains 4 positive integers: N (<= 500) - the number of cities (and the cities are numbered from 0 to N-1), M - the number of roads, C1 and C2 - the cities that you are currently in and that you must save, respectively. The next line contains N integers, where the i-th integer is the number of rescue teams in the i-th city. Then M lines follow, each describes a road with three integers c1, c2 and L, which are the pair of cities connected by a road and the length of that road, respectively. It is guaranteed that there exists at least one path from C1 to C2.

** Output **

&emsp;&emsp;For each test case, print in one line two numbers: the number of different shortest paths between C1 and C2, and the maximum amount of rescue teams you can possibly gather.  All the numbers in a line must be separated by exactly one space, and there is no extra space allowed at the end of a line.

** Sample Input **
    
    
    5 6 0 2
    1 2 1 5 3
    0 1 1
    0 2 2
    0 3 1
    1 2 1
    2 4 1
    3 4 1
    

** Sample Output **
    
    
    2 4
    

* * *

&emsp;&emsp;分析：此题需要求出最短路径的条数和救援队的族大数目，先用邻接矩阵的表示法构造出图，然后用单源最短路径算法求出最短路径，在用深度搜索算法找出最短路径的数目。

code：

    
```C++   
    #include<iostream>
    #include<cstring>
    using namespace std;
    
    const int N = 1000;
    const int INF = 100000000;
    
    //图的邻接矩阵
    int road[N][N];
    
    //标记某一节点是否被访问过
    bool isVisited[N];
    
    //记录每一节点的权值
    int team_num[N];
    
    int dis[N];
    
    //最短路径的数目
    int path_num = 0;
    
    //最大的医疗队的数目
    int g_max_team_num = 0;
    
    void initData()
    {
      int i, j;
      for (i = 0; i<N; i++)
      {
        isVisited[i] = false;
        team_num[i] = 0;
        dis[i] = INF;//初始化为无穷大.
        for (j = 0; j<N; j++)
        {
          if (j != i)
          {
            road[i][j] = INF;
            road[j][i] = INF;
          }
          //当i==j的时候，一定要赋值为0，否则当起点src和终点des相同的时候，test case是过不了的。
          else
            road[i][j] = 0;
        }
      }
    }
    
    //单源最短路径算法。
    void Dijstra(int n, int src, int des)
    {
      int i, j;
      for (i = 0; i<n; i++)
        dis[i] = road[src][i];
      isVisited[src] = true;
    
      for (i = 0; i<n - 1; i++)//最多循环n-1次就足够了，选n-1个最小值。
      {
        int minDis = INF;
        int cur = 0;
        for (j = 0; j<n; j++)
          if (!isVisited[j] && dis[j]<minDis)
          {
            minDis = dis[j];
            cur = j;
          }
        if (minDis == INF) //已经完成了连通路径的遍历。
          return;
        //dis[cur]为dis数组中的最小值，访问节点cur.
        isVisited[cur] = true;
        //更新Dis数组的内容.
        for (j = 0; j<n; j++)
          if (road[cur][j] <INF && dis[j] > dis[cur] + road[cur][j])
            dis[j] = dis[cur] + road[cur][j];
      }
    }
    
    //深度搜索来得到最短路径的数目。
    void dfs(int n, int cId, int des, int curDis, int curTeamsNum)
    {
      isVisited[cId] = true;
      if (cId == des)
      {
        if (curDis == dis[des]) //找到一条最短路径
        {
          path_num++;//最短路径数目加1
          if (curTeamsNum > g_max_team_num)
            g_max_team_num = curTeamsNum;
        }
        return;
      }
      if (curDis > dis[des]) //当前的路径长度已经超过最短路径,就没有必要继续搜索了。
        return;
      //从城市cId开始搜索
      for (int i = 0; i<n; i++)
      {
    
        if (!isVisited[i] && road[cId][i] < INF)//如果城市i没有被访问过，且cId到i连通。      
        {
          //isVisited[i] = true;
          dfs(n, i, des, curDis + road[cId][i], curTeamsNum + team_num[i]);
          isVisited[i] = false;
        }
    
        /*
        //这样的剪枝比上一种更加强大。
        if(dis[cId] + road[cId][i] == dis[i])
        dfs(n,i,des,curDis+road[cId][i],curTeamsNum+team_num[i]);
        */
      }
    }   
    
    int main()
    {
      int i, j, n, m, c1, c2, L, src, des;
        
      initData();
      ios::sync_with_stdio(false);
      cin >> n >> m >> src >> des;
      for (i = 0; i<n; i++)
        cin >> team_num[i];
      for (i = 0; i<m; i++)
      {
        cin >> c1 >> c2 >> L;
        road[c1][c2] = L;
        road[c2][c1] = L;
      }
    
      Dijstra(n, src, des);
    
      //重置各city的被访问状态。
      for (i = 0; i<n; i++)
        isVisited[i] = false;
    
      dfs(n, src, des, 0, team_num[src]);
    
      cout << path_num << " " << g_max_team_num << endl;
      return 0;
    }

```

很久没写过了，今天回来，慢慢训练，慢慢提升。  

