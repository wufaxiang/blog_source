
---
title: 09-3. Hashing - Hard Version
date: 2015-02-06 14:54:12
tags: 
  - PAT
  - 数据结构
  - Hash
category: PAT
---

题目：  

&emsp;&emsp;Given a hash table of size N, we can define a hash function H(x) = x%N. Suppose that the linear probing is used to solve collisions, we can easily obtain the status of the hash table with a given sequence of input numbers.

&emsp;&emsp;However, now you are asked to solve the reversed problem: reconstruct the input sequence from the given status of the hash table. Whenever there are multiple choices, the smallest number is always taken.
<!-- more -->
** Input Specification: **

&emsp;&emsp;Each input file contains one test case. For each test case, the first line contains a positive integer N (<=1000), which is the size of the hash table.The next line contains N integers, separated by a space. A negative integer represents an empty cell in the hash table. It is guaranteed that all the non- negative integers are distinct in the table.

** Output Specification: **

&emsp;&emsp;For each test case, print a line that contains the input sequence, with the numbers separated by a space. Notice that there must be no extra space at the end of each line.

** Sample Input: **
    
    
    11
    33 1 13 12 34 38 27 22 32 -1 21
    

** Sample Output: **
    
    
    1 13 12 21 33 34 38 27 22 32
    

* * *

&emsp;&emsp;分析：上一个星期去干别的了，当时剩了这最后一道题了，当时一直想不来，实在是无奈，所以过了一个星期才回来继续把它完成，在听完姥姥的讲解后，的确是很需要思考和分
析能力的一道题。  
题目的意思很简单，仔细阅读后（虽然也花了蛮久的，英语太差，怪我咯），还是能明白题意的，就是hash表的逆过程，也就是给出hash表序列和hash函数，需要你
求出原始的序列，需要注意的是当有多个时，小的在前面（the smallest number is always taken.）。  
这道题的代码不是自己写的，相似度很高，自己还需修行，太菜呀。  
解题思路：  
1. 把Hash表的序列保存到数组中（我的习惯使用了vector，数组也一样的）  
2. 计算入度，即冲突次数，并建立有向图，把下标当顶点。从Key%N 到 当前下标 - 1的顶点都指向当前下标，因为这些点都影响当前顶点。  
3. 拓扑排序，把度为0的顶点放入最小堆中，STL可以用优先队列。（STL真的是很好的工具）  
4. 找出度为0且值最小的顶点，扫描与该点相连的顶点，入度 - 1，如果入度变0，则加入最小堆或者优先队列。  
5. 取最小堆顶点或者取队列首元素的时候，把该顶点的Key存入数组，当然你也可以直接输出。

代码：

    
```C++    
    #include<iostream>
    #include<vector>
    #include<queue>
    #include<functional>
    
    using namespace std;
    
    int main()
    {
    	int N;
    	cin >> N;		//输入哈希表的大小
    	int *Hash = new int[N];			//Hash数组
    	int *Degree = new int[N];		//入度数组
    	vector<vector<int>> G(N);		//无向图  二维vector容器
    	vector<int> Ans;				//输出序列
    
    	//输入hash序列
    	for (int i = 0; i < N; i++){
    		cin >> Hash[i];
    		if (Hash[i] > 0)
    			Degree[i] = 0;
    		else
    			Degree[i] = -1;			//如果小于0，入度记为-1，表示没有元素
    	}
    
    	//计算入度并建立无向图
    	for (int i = 0; i < N; i++){
    		if (Hash[i] < 0)
    			continue;
    		int curPos = i;				//当前坐标
    		int hashPos = Hash[i] % N;	//Hash后的坐标
    		Degree[i] = (curPos - hashPos + N) % N;	//计算入度，也就是冲突的次数
    		for (int j = 0; j < Degree[i]; j++)
    			G[(hashPos + j + N) % N].push_back(i);
    	}
    
    	//拓扑排序
    	typedef pair<int, int> PAIR;
    	priority_queue<PAIR, vector< PAIR >, greater< PAIR >> q;		//优先队列
    	for (int i = 0; i < N; i++){
    		if (Degree[i] == 0){
    			q.push(PAIR(Hash[i], i));
    		}
    	}
    	
    	while (!q.empty())
    	{
    		PAIR p = q.top();                       //每次取出当前入度为0的顶点中Key最小的
    		int V = p.second;                       //second为顶点
    		Ans.push_back(p.first);                 //first为该顶点的Key
    		q.pop();
    		for (int i = 0; i < G[V].size(); i++)    //扫描关联顶点，入度处理
    			if (--Degree[G[V][i]] == 0)
    				q.push(PAIR(Hash[G[V][i]], G[V][i]));
    	}
    	//输出
    	cout << Ans[0];
    	for (int i = 1; i < Ans.size(); i++)
    		cout << ' ' << Ans[i];
    
    	return 0;
    }
    
```    

  

&emsp;&emsp;总结：最后一道题也结束了，通过这门课，pat的这些题，真的学到了很多东西，也发现了自己真的好菜呀，不过所有人一开始都是不懂得嘛，慢慢学。当然，同时发现，自己
需要学的东西还有很多，真的好难呀，前面的这些题，真正自己做出来的真的没几个，很多都是通过查阅，google，百度呀出来的，所以还要好好修行，但也正是查阅过程
中，感觉还是学会很多的知识的，比如STL，这真的是一个强大的工具，当然这个也不是万能的哈，有些时候还是要自己写最基础的算法的。 ^_^  

