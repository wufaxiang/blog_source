---
title: 单源最短路径之Dijkstra算法
date: 2015-02-12 15:53:12
tags: 
  - Dijkstra
  - 图
  - 数据结构
category: 数据结构
---

&emsp;&emsp;Dijkstra算法是典型最短路算法，用于计算一个节点到其他所有节点的最短路径。主要特点是以起始点为中心向外层层扩展，直到扩展到终点为止。Dijkstra算法能得出最短路径的最优解，但由于它遍历计算的节点很多，所以效率低。  
算法思想：  
设G=(V,E)是一个带权有向图，把图中顶点集合V分成两组，第一组为已求出最短路径的顶点集合（用S表示，初始时S中只有一个源点，以后每求得一条最短路径 ,
就将 加入到集合S中，直到全部顶点都加入到S中，算法就结束了），第二组为其余未确定最短路径的顶点集合（用U表示），按最短路径长度的递增次序依次把第二组的顶点
加入S中。在加入的过程中，总保持从源点v到S中各顶点的最短路径长度不大于从源点v到U中任何顶点的最短路径长度。此外，每个顶点对应一个距离，S中的顶点的距离就
是从v到此顶点的最短路径长度，U中的顶点的距离，是从v到此顶点只包括S中的顶点为中间顶点的当前最短路径长度。  
Dijkstra算法具体步骤  
（1）初始时，S只包含源点，即S＝，v的距离为0。U包含除v外的其他顶点，U中顶点u距离为边上的权（若v与u有边）或 ）（若u不是v的出边邻接点）。  
（2）从U中选取一个距离v最小的顶点k，把k，加入S中（该选定的距离就是v到k的最短路径长度）。  
（3）以k为新考虑的中间点，修改U中各顶点的距离；若从源点v到顶点u（u
U）的距离（经过顶点k）比原来距离（不经过顶点k）短，则修改顶点u的距离值，修改后的距离值的顶点k的距离加上边上的权。  
（4）重复步骤（2）和（3）直到所有顶点都包含在S中。  
<!-- more -->
举个栗子：  
如下图，设A为源点，求A到其他各顶点（B、C、D、E、F）的最短路径。线上所标注为相邻线段之间的距离，即权值。（注：此图为随意所画，其相邻顶点间的距离与图中
的目视长度不能一一对等）

![](http://img.blog.csdn.net/20150212155834237?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

步骤：  
1.S集合中：选入A，此时S=<A>  
此时最短路径A→A=0 以A为中间点，从A开始找  
U集合中：U=<B、C、D、E、F>  
A→B=6，A→C=3， A→其它U中的顶点=∞， 发现A→C=3权值为最短  
2.S集合中：选入C，此时S=<A、C>  
此时最短路径A→A=0，A→C=3以C为中间点，从A→C=3这条最短路径开始找  
U集合中：U=<B、D、E、F>  
A→C→B=5(比上面第一步的A→B=6要短) 此时到D权值更改为A→C→B=5， A→C→D=6， A→C→E=7，
A→C→其它U中的顶点=∞，发现A→C→B=5权值为最短  
3.S集合中：选入B，此时S=<A、C、B>  
此时最短路径A→A=0，A→C=3，A→C→B=5 以B为中间点  
U集合中：U=<E、F>  
A→C→D→E=8(比上面第二步的A→C→E=7要长)此时到E权值更改为A→C→E=7，A→C→D→F=9
发现A→C→E=7权值为最短从A→C→B=5这条最短路径开始找  
4.S集合中：选入D，此时S=<A、C、B、D>  
此时最短路径A→A=0，A→C=3，A→C→B=5，A→C→D=6 以D为中间点， 从A→C→D=6这条最短路径开始找  
U集合中：U=<E、F>  
A→C→D→E=8(比上面第二步的A→C→E=7要长)此时到E权值更改为A→C→E=7，A→C→D→F=9 发现A→C→E=7权值为最短  
5.S集合中：选入E，此时S=<A、C、B、D、E>  
此时最短路径A→A=0，A→C=3，A→C→B=5，A→C→D=6，A→C→E=7，以E为中间点， 从A→C→E=7这条最短路径开始找  
U集合中：U=<F>  
A→C→E→F=12(比上面第四步的A→C→D→F=9要长)此时到F权值更改为A→C→D→F=9 发现A→C→D→F=9权值为最短  
6.S集合中：选入F，此时S=<A、C、B、D、E、F>  
此时最短路径A→A=0，A→C=3， A→C→B=5， A→C→D=6， A→C→E=7，A→C→D→F=9  
U集合中：U集合已空，查找完毕。

  

code：

```C++
    
    const int  MAXINT = 32767;
    const int MAXNUM = 10;
    int dist[MAXNUM];
    int prev[MAXNUM];
    
    int A[MAXUNM][MAXNUM];
    
    void Dijkstra(int v0)
    {
      　　bool S[MAXNUM];                                  // 判断是否已存入该点到S集合中
          int n=MAXNUM;
      　　for(int i=1; i<=n; ++i)
     　　 {
          　　dist[i] = A[v0][i];
          　　S[i] = false;                                // 初始都未用过该点
          　　if(dist[i] == MAXINT)    
                　　prev[i] = -1;
     　　     else 
                　　prev[i] = v0;
       　　}
       　 dist[v0] = 0;
       　 S[v0] = true; 　　
     　　 for(int i=2; i<=n; i++)
     　　 {
           　　int mindist = MAXINT;
           　　int u = v0; 　　                            // 找出当前未使用的点j的dist[j]最小值
          　　 for(int j=1; j<=n; ++j)
          　　    if((!S[j]) && dist[j]<mindist)
          　　    {
             　　       u = j;                             // u保存当前邻接点中距离最小的点的号码 
             　 　      mindist = dist[j];
           　　   }
           　　S[u] = true; 
           　　for(int j=1; j<=n; j++)
           　　    if((!S[j]) && A[u][j]<MAXINT)
           　　    {
               　    　if(dist[u] + A[u][j] < dist[j])     //在通过新加入的u点路径找到离v0点更短的路径  
               　    　{
                       　　dist[j] = dist[u] + A[u][j];    //更新dist 
                       　　prev[j] = u;                    //记录前驱顶点 
                　　    }
            　    　}
       　　}
    }
```
