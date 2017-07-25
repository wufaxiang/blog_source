
---
title: File Transfer
date: 2014-12-31 14:11:12
tags: 
  - PAT
category: PAT
---


&emsp;&emsp;We have a network of computers and a list of bi-directional connections. Each of these connections allows a file transfer from one computer to another. Is it possible to send a file from any computer on the network to any other?
<!-- more -->
** Input Specification: **

&emsp;&emsp;Each input file contains one test case. For each test case, the first line contains N (2<=N<=10  4  ), the total number of computers in a network. Each computer in the network is then represented by a positive integer between 1 and N. Then in the following lines, the input is given in the format:
  
    
    I c1 c2  
    

where ** I ** stands for inputting a connection between ** c1 ** and ** c2 **; or   
    
    C c1 c2    
    

where ** C ** stands for checking if it is possible to transfer files between ** c1 ** and ** c2 ** ; or
  
    
    S
    

where ** S ** stands for stopping this case.

** Output Specification: **

For each ** C ** case, print in one line the word "yes" or "no" if it is possible or impossible to transfer files between ** c1 ** and ** c2 ** , respectively. At the end of each case, print in one line "The network is connected." if there is a path between any pair of computers; or "There are k components." where ** k ** is the number of connected components in this network.

Sample Input 1:

    
    
    5
    C 3 2
    I 3 2
    C 1 5
    I 4 5
    I 2 4
    C 3 5
    S
    

Sample Output 1:

    
    
    no
    no
    yes
    There are 2 components.
    

Sample Input 2:

    
    
    5
    C 3 2
    I 3 2
    C 1 5
    I 4 5
    I 2 4
    C 3 5
    I 1 3
    C 1 5
    S
    

Sample Output 2:

    
    
    no
    no
    yes
    yes
    The network is connected.

解释下题目的意思：

这道题就是一个并查的问题，也就是判断2个数是否属于同一个集合

输入格式：

第一行先输入结点的总数量

接着就是输入需要进行的操作，有三种：C,I,S

C表示检查2个数是不是在一个集合，I 就是将后面的两个数合并到为一个集合，S就是停止了

输出的话：

就是先判断那些进行check的集合，最后还要输出有多少个集合，要注意只有一个是，输出的应为  The network is connected.

  

一开始做这道题时，在想到底改用什么方法来完成，其实这个问题很简单，我们用数学思维的方式一下就能做出来，但是要如何用编程的方式来解决？

我先准备像老师讲的用数组来储存结点的方式来做题，就是采用树的形式，一个结点包括data和 parent两个元素，最后也的确完成了。

后来发现一个大神，直接用数组的方式就解决了，粘上链接:http://www.cnblogs.com/clevercong/p/4192953.html

发现这方法实在是太赞了

直接上代码：

    
```C++    
    #include <iostream>
    #define ElementType int
    using namespace std;
    
    int *S;
    int Find(ElementType X)
    {
    	if (S[X] == X)
    		return X;
    	return S[X] = Find(S[X]);
    }
    
    void Union(ElementType X1, ElementType X2)
    {
    	int Root1, Root2;
    	Root1 = Find(X1);
    	Root2 = Find(X2);
    	if (Root1 != Root2)
    	{
    		if (S[Root1] < S[Root2])
    			S[Root2] = Root1;
    		else
    			S[Root1] = Root2;
    	}
    }
     int main()
     {
    	 int num;
    	 cin >> num;
    	 char choose;
    	 int c1, c2;
    	 S = new int[num + 1];
    	 for (int i = 0; i <= num; i++)   // 初始化
    		S[i] = i;
    	 while (1)
    	 {
    		 cin >> choose;
    		 if (choose == 'S')
    			 break;
    		 cin >> c1 >> c2;
    		 if (choose == 'I')
    			 Union(c1, c2);
    		 if (choose == 'C')
    		 {
    			 if (Find(c1) == Find(c2))
    				 cout << "yes" << endl;
    			 else
    				 cout << "no" << endl;
    		 }
    	 }
    	int icount = 0;
    	for (int i = 1; i <= num; i++)
    		if (S[i] == i)  
    			icount++;
    	if (icount == 1)
    		cout << "The network is connected." << endl;
    	else
    		cout << "There are " << icount << " components." << endl;
    	return 0;
     }
```
  
感谢大神的分享！！！

