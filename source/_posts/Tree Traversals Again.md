
---
title: Tree Traversals Again
date: 2014-12-29 21:54:12
tags: 
  - PAT
  - 数据结构
  - Tree
category: PAT
---

先粘下题目

&emsp;&emsp;An inorder binary tree traversal can be implemented in a non-recursive way with a stack. For example, suppose that when a 6-node binary tree (with the keys numbered from 1 to 6) is traversed, the stack operations are: push(1); push(2); push(3); pop(); pop(); push(4); pop(); pop(); push(5); push(6); pop(); pop(). Then a unique binary tree (shown in Figure 1) can be generated from this sequence of operations. Your task is to give the postorder traversal sequence of this tree.

  

![](http://img.blog.csdn.net/20141229215627357?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)  
<!-- more -->
** Input Specification: **

&emsp;&emsp;Each input file contains one test case. For each case, the first line contains a positive integer N (<=30) which is the total number of nodes in a tree (and hence the nodes are numbered from 1 to N). Then 2N lines follow, each describes a stack operation in the format: "Push X" where X is the index of the node being pushed onto the stack; or "Pop" meaning to pop one node from the stack.

** Output Specification: **

&emsp;&emsp;For each test case, print the postorder traversal sequence of the corresponding tree in one line. A solution is guaranteed to exist. All the numbers must be separated by exactly one space, and there must be no extra space at the end of the line.

** Sample Input: **
    
    
    6
    Push 1
    Push 2
    Push 3
    Pop
    Pop
    Push 4
    Pop
    Pop
    Push 5
    Push 6
    Pop
    Pop
    

** Sample Output: **
    
    
    3 4 2 6 5 1
    

  

  

解释下题目的意思：

就是采用了堆栈的形式获取树的中序遍历

输入就是  第一行是结点的总数，后面就是栈的进出操作来获取中序遍历，我们需要的就是将通过栈操作得到的唯一树进行后序遍历，并将其输出

  

解决方法就是：

当时Push 操作时，就增加一个标志变量 设为 1  然后入栈

当为Pop操作时，：

如果是1，弹出来变成2压回栈；

如果是2，则弹出，放入存放结果的vector中，重复这一过程，直到栈顶flag为1。

所有PUSH与POP操作执行完毕时，输出vector内的数据和stack中的数据即可。注意要处理最后的空格。

  

代码如下：

    
```C++  
    #include<iostream>
    #include<vector>
    #include<string>
    #include<stack>
    using namespace std;
    
    typedef struct node{
    	int data;
    	int flag;
    	node(int d,int f){
    		data = d;
    		flag = f;
    	}
    }Node;
    
    int main()
    {
    	//输入也第一行
    	int n;
    	cin >> n;
    	//输入操作的n行
    	string operate;
    	int x;
    	stack<Node> sta;
    	vector<int> vec;
    	for (int i = 0; i < 2 * n; i++)
    	{
    		cin >> operate;
    		if (operate == "Push")
    		{
    			cin >> x;
    			sta.push(Node(x, 1));
    		}
    		if (operate == "Pop")
    		{
    			Node node = sta.top();
    			sta.pop();
    			if (node.flag == 1)
    			{
    				node.flag = 2;
    				sta.push(node);
    			}
    			else if (node.flag == 2)
    			{
    				vec.push_back(node.data);
    				//sta.pop();
    				while (sta.top().flag == 2)
    				{
    					vec.push_back(sta.top().data);
    					sta.pop();
    				}
    				if (sta.size() != 0)
    				{
    					node = sta.top();
    					node.flag = 2;
    					sta.pop();
    					sta.push(node);
    				}
    			}
    		}
    	}
    
    	for (int i = 0; i < vec.size(); i++)
    	{
    		cout << vec[i] << " ";
    	}
    
    	while (!sta.empty())
    	{
    		cout << sta.top().data;
    		sta.pop();
    		if (sta.size() != 0)
    		{
    			cout << " ";
    		}
    	}
    
    	return 0;
    }
    

```

