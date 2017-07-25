---
title: Huffman Codes
date: 2015-01-02 21:27:12
tags: 
  - PAT
  - 霍夫曼编码
category: PAT
---

首先，不多说啦，粘题目：

&emsp;&emsp;In 1953, David A. Huffman published his paper "A Method for the Construction of Minimum-Redundancy Codes", and hence printed his name in the history of computer science. As a professor who gives the final exam problem on Huffman codes, I am encountering a big problem: the Huffman codes are NOT unique. For example, given a string "aaaxuaxz", we can observe that the frequencies of the characters 'a', 'x', 'u' and 'z' are 4, 2, 1 and 1, respectively. We may either encode the symbols as {'a'=0, 'x'=10, 'u'=110, 'z'=111}, or in another way as {'a'=1, 'x'=01, 'u'=001, 'z'=000}, both compress the string into 14 bits. Another set of code can be given as {'a'=0, 'x'=11, 'u'=100, 'z'=101}, but {'a'=0, 'x'=01, 'u'=011, 'z'=001} is NOT correct since "aaaxuaxz" and "aazuaxax" can both be decoded from the code 00001011001001. The students are submitting all kinds of codes, and I need a computer program to help me determine which ones are correct and which ones are not.
<!-- more -->
** Input Specification: **

&emsp;&emsp;Each input file contains one test case. For each case, the first line gives an integer N (2 <= N <= 63), then followed by a line that contains all the N distinct characters and their frequencies in the following format:

    
    
    c[1] f[1] c[2] f[2] ... c[N] f[N]
    

where c[i] is a character chosen from {'0' - '9', 'a' - 'z', 'A' - 'Z', '_'}, and f[i] is the frequency of c[i] and is an integer no more than 1000. The next line gives a positive integer M (<=1000), then followed by M student submissions. Each student submission consists of N lines, each in the format:

    
    
    c[i] code[i]
    

where c[i] is the i-th character and code[i] is a string of '0's and '1's.

** Output Specification: **

For each test case, print in each line either “Yes” if the student’s submission is correct, or “No” if not.

Sample Input:

    
    
    7
    A 1 B 1 C 1 D 3 E 3 F 6 G 6
    4
    A 00000
    B 00001
    C 0001
    D 001
    E 01
    F 10
    G 11
    A 01010
    B 01011
    C 0100
    D 011
    E 10
    F 11
    G 00
    A 000
    B 001
    C 010
    D 011
    E 100
    F 101
    G 110
    A 00000
    B 00001
    C 0001
    D 001
    E 00
    F 10
    G 11
    

Sample Output:

    
    
    Yes
    Yes
    No
    No

  

  

开始分析题目：

这道题是一个构造霍夫曼树求WPL的问题

接着我们观察输入输出

输入：

·  第一行老规矩，结点的数目

后面跟着的就是一行输入，就是包括每个结点的data和权重weight

后面的一行输入的就是测试学生的个数

继续接着就是每个学生的 datra 和 它们的 霍夫曼编码

输出：

就是对测试学生的输入进行判断，正确就输出“Yes”，否则输入“No”

  

首先我们需要考虑怎么样去解决这个问题，要判断测试学生的数据是否正确，我们想到的方法肯定是通过求出它们的最小WPL（最小带权路径长度），我们最常规的方法就是建
立最小堆，建立霍夫曼树，再求WPL.当然不要忘了，还要检测测试代码的准确性，也就是前缀码的问题。

但是我今天换一个方式来求出这道题，不需要建堆，也不需要建树，我们使用系统给的queen头文件里优先队列  priority_queue  就可解决

先解释下优先队列：附个链接： _ http://www.cppblog.com/shyli/archive/2007/04/06/21366.html _

优先队列就是出队时是按照一定优先级来出队的

用优先队列就可以直接根据输入的结点信息将WPL求出来，具体方法请参考后面源码中

接着我们需要的是通过比较测试学生给的测试案例求出的WPL 跟 我们开始求出的WPL 进行比较，如果不同，那说明肯定是错误的

（通过测试学生案例的霍夫曼树来求出WPL的算法具体看源码，这里不解释）

判断完了WPL,如果相等，就是对的么？那不一定，我们还需要对测试学生案例的霍夫曼码进行分析，霍夫曼码是否正确，这个就判断是否有相同前缀码，如果没有，那么就可
以判定这个测试学生案例的 霍夫曼树 是正确的 ，这道题也就结束了。

  

最后附上源码：

  

```C++    
    
    #include<iostream>
    #include<queue>      
    #include <algorithm> //要使用排序函数，所以需引入此头文件
    #include<map>
    #include <functional>
    #include<string>
    #include<vector>
    using namespace std;
    
    // 用PAIR来代替pair<char, string>
    typedef pair<char, string> PAIR;
    
    // SortMethod函数决定是从小到大排，还是从大到小排
    // 还有按什么内容排，这里是按编码的长度排序
    int SortMethod(const PAIR& x, const PAIR& y)
    {
    	return x.second.size() < y.second.size();
    }
    
    int main()
    {
    	int n;
    	cin >> n;       //输入第一行需要输入的结点数目
    
    	char *d = new char[n];						
    	int *w = new int[n];						//定义2个数组，分别存放结点的 data 和 权重 weiht
    	map<char, int>MyMap;                        //定义一个map容器
    	// 使用优先级队列模拟“堆”
    	priority_queue<int, vector<int>,greater<int> >pq;
    	
    	//输入结点和权重
    	for (int i = 0; i < n; i++)
    	{
    		cin >> d[i] >> w[i];
    		MyMap[d[i]] = w[i];
    		pq.push(w[i]);                    //添加元素到队列
    	}
    
    	//计算WPL的值
    	int myWpl = 0;
    	while (!pq.empty())                   //队列不为空
    	{
    		int top = pq.top();           //定义一个变量表示队头
    		pq.pop();					 //出队
    		if (!pq.empty())             //队列非空
    		{
    			int top2 = pq.top();     
    			pq.pop();                 //出队
    			pq.push(top + top2);         //将2次的top之和入队
    			int m = top + top2;        
    			myWpl += m;                  //求出wpl
    		}
    	}
    
    	//输入测试的数据
    	int checkNum;
    	cin >> checkNum;
    	for (int i = 0; i < checkNum; i++)
    	{
    		int wpl = 0;
    		char ch;                    //测试的data
    		string s;                   //测试的Huffman Code
    		vector<PAIR> checkVec;            //定义一个Vector存放测试数据
    		for (int j = 0; j < n; j++)
    		{
    			cin >> ch >> s;
    			checkVec.push_back(make_pair(ch, s));       //vector 中添加元素
    			wpl += (s.size() * MyMap[ch]);
    		}
    		//按编码长度进行排序
    		sort(checkVec.begin(), checkVec.end(), SortMethod);
    		if (wpl != myWpl)
    		{
    			cout << "No" << endl;
    			continue;
    		}
    		else
    		{
    			bool flag = true;
    			for (int i = 0; i < n; i++)
    			{
    				string tmp = checkVec[i].second;
    				for (int j = i+1; j < n; j++)
    				{
    					if (checkVec[j].second.substr(0, tmp.size()) == tmp)
    						flag = false;
    				}
    			}
    			if (flag == true)
    				cout << "Yes" << endl;
    			else
    				cout << "No" << endl;
    			continue;
    		}
    		cout << "Yes" << endl;
    	}
    	return 0;
    }
```
  
算法中使用了很多系统自带的算法或函数，不一一解释，请找度娘，*_*

  

