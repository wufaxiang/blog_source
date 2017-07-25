---
title: List Leaves
date: 2014-12-27 16:36:12
tags: 
  - PAT
  - 数据结构
  - Tree
category: PAT
---

先粘下题目

Given a tree, you are supposed to list all the leaves in the order of top down, and left to right.

** Input Specification: **

Each input file contains one test case. For each case, the first line gives a positive integer N (<=10) which is the total number of nodes in the tree -- and hence the nodes are numbered from 0 to N-1. Then N lines follow, each corresponds to a node, and gives the indices of the left and right children of the node. If the child does not exist, a "-" will be put at the position. Any pair of children are separated by a space.
<!-- more -->
** Output Specification: **

For each test case, print in one line all the leaves' indices in the order of top down, and left to right. There must be exactly one space between any adjacent numbers, and no extra space at the end of the line.

** Sample Input: **
    
    
    8
    1 -
    - -
    0 -
    2 7
    - -
    - -
    5 -
    4 6
    

** Sample Output: **
    
    
    4 1 5
    

这道题目要注意下输出格式

先解释下题目的意思，

输入格式：

第一行是结点的总数n

后面每行表示的是每个结点 的左孩子和右孩子的编号，比如下面的第一行就是表示 0 这个结点的左孩子是1，右孩子为空

输出格式：

就是按照从上至下，从左至右的顺序（也就是层次遍历）输出叶子结点，注意最后一个后面不能有空格

  

以下就是代码，自己测试是可以通过，但是测试时PAT显示有两个测试点说是段错误，后面再进行改进，也希望大家帮忙改进

    
```C++    
    #include<iostream>
    #include<vector>
    #include<queue>
    using namespace std;
    
    struct TreeNode
    {
    	TreeNode *left;
    	TreeNode *right;
    	int data;
    	TreeNode()
    	{
    	}
    	TreeNode(int d, TreeNode *l, TreeNode *r)
    	{
    		data = d;
    		left = l;
    		right = r;
    	}
    };
    
    typedef struct myNode
    {
    	char left;
    	char right;
    	myNode(char l, char r)   //构造函数
    	{
    		left = l;
    		right = r;
    	}
    }myNode;
    
    int main()
    {
    	vector<myNode> vec;
    	vector<TreeNode> tree;
    	int n;   //输入第一行的整数
    	cin >> n;
    	char a, b;
    	//输入后面的n行数据并存储到 vec 中
    	for (int i = 0; i < n; i++)
    	{
    		cin >> a >> b;
    		vec.push_back(myNode(a, b));
    	}
    
    	//生成树的n个结点 放入 tree 中
    	for (int i = 0; i < n; i++)
    	{
    		tree.push_back(TreeNode(i, NULL, NULL));
    	}
    
    	//建立一颗二叉树
    	for (int i = 0; i < n; i++)
    	{
    		if (vec[i].left != '-')
    		{
    		 tree[i].left = &tree[vec[i].left - '0'];
    		}
    		else
    		{
    			tree[i].left = NULL;
    		}
    		if (vec[i].right != '-')
    		{
    			tree[i].right = &tree[vec[i].right - '0'];
    		}
    		else
    		{
    			tree[i].right = NULL;
    		}
    	}
    
    	//寻找根结点
    	vector<bool> flag;
    	for (int i = 0; i < n; i++)
    		flag.push_back(false);
    	TreeNode root;
    	for (int i = 0; i < n; i++)
    	{
    		if (vec[i].left != '-')
    			flag[vec[i].left - '0'] = true;
    		if (vec[i].right != '-')
    			flag[vec[i].right - '0'] = true;
    	}
    	for (int i = 0; i < n; i++)
    	{
    		if (flag[i] == false)
    		{
    			root.data = i;
    			root.left = &tree[vec[i].left - '0'];
    			root.right = &tree[vec[i].right - '0'];
    		}
    	}
    
    	//层次遍历，使用队列
    	queue<TreeNode> que;
    	que.push(root);
    	TreeNode temp;
    	
    	//将所有的结点入队
    	for (int i = 1; i <= n; i++)
    	{
    		temp = que.front();
    		if (temp.left != NULL)
    		{
    			que.push(tree[temp.left->data]);						
    		}
    		if (temp.right != NULL)
    			que.push(tree[temp.right->data]);
    		if (temp.left == NULL && temp.right == NULL)
    		{
    			cout << temp.data;
    			if (i != n )
    				cout << " ";
    		}
    		que.pop();
    	}
    
    	return 0;
    }

  
  
```
