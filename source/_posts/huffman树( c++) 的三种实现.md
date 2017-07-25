---
title: huffman树( c++) 的三种实现
date: 2014-12-31 14:05:12
tags: 
  - C++ 
  - 霍夫曼树
category: 数据结构
---

今天转一篇huffman树的实现代码

原文链接：http://www.icourse163.org/learn/zju-93001#/learn/forumdetail?pid=532240

还有一个最小建堆的资料，不过是英文版的（比较难过），链接：http://www.geeksforgeeks.org/greedy-algorithms-set-3-huffman-coding/
<!-- more -->
代码如下：

    
```C++    
    /* huffman.cc
     Copyright(C) 2014 Liu Xinyu()
    
    * /
    
    /*
    * D.A. Huffman, "A Method for the Construction of Minimum-Redundancy Codes",
    * Proceedings of the I.R.E., September 1952, pp 1098–1102.
    */
    
    #include <algorithm> /* builtin swap etc. */
    #include <vector> /* array of huffman trees. */
    #include <string> /* to store variable-length coding/decoding result. */
    #include <map>    /* to store code table. */
    #include <queue>
    #include <cstdio>
    
    using namespace std;
    
    /* Definition of Huffman tree node. */
    struct Node 
    {
    	int w;
    	char c;
    	Node *left, *right;
    };
    
    typedef vector<Node*> Nodes;
    typedef map<char, string> CodeTab;
    
    int isleaf(Node* a) 
    { 
    	return (!a->left) && (!a->right); 
    }
    
    Node* leaf(char c, int w) 
    {
    	Node* n = new Node();
    	n->w = w;
    	n->c = c;
    	n->left = n->right = NULL;
    	return n;
    }
    
    void release(Node* t) {
    	if (t) {
    		release(t->left);
    		release(t->right);
    		delete t;
    	}
    }
    
    Node* merge(Node* a, Node* b) 
    {
    	Node* n = new Node();
    	n->w = a->w + b->w;
    	n->left = a;
    	n->right = b;
    	return n;
    }
    
    bool lessp(Node* a, Node* b) 
    { 
    	return a->w < b->w; 
    }
    
    bool greaterp(Node* a, Node* b) 
    { 
    	return b->w < a->w; 
    }
    
    Node* max(Node* a, Node* b) 
    { 
    	return lessp(a, b) ? b : a; 
    }
    
    void swap(Nodes& ts, int i, int j, int k) 
    {
    	swap(ts[i], ts[ts[j] < ts[k] ? k : j]);
    }
    
    /*
    * Method 1, Build the Huffman tree by repeatedly extracting the 2
    * trees with the smallest weight.
    */
    Node* huffman(Nodes ts) 
    {
    	int n;
    	while ((n = ts.size()) > 1) 
    	{
    		for (int i = n - 3; i >= 0; --i)
    			if (lessp(ts[i], max(ts[n - 1], ts[n - 2])))
    				swap(ts, i, n - 1, n - 2);
    		ts[n - 2] = merge(ts[n - 1], ts[n - 2]);
    		ts.pop_back();
    	}
    	return ts.front();
    }
    
    /*
    * Method 2, Build the Huffman tree by using Heap.
    * Repeatedly pop 2 trees from the heap for merging.
    */
    Node* pop(Nodes& h) 
    {
    	Node* m = h.front();
    	pop_heap(h.begin(), h.end(), greaterp);
    	h.pop_back();
    	return m;
    }
    
    void push(Node* t, Nodes& h) 
    {
    	h.push_back(t);
    	push_heap(h.begin(), h.end(), greaterp);
    }
    
    Node* huffman1(Nodes ts) 
    {
    	make_heap(ts.begin(), ts.end(), greaterp);
    	while (ts.size() > 1) 
    	{
    		Node* t1 = pop(ts);
    		Node* t2 = pop(ts);
    		push(merge(t1, t2), ts);
    	}
    	return ts.front();
    }
    /*
    * Method 3, If the symbol-weight list is ordered, Huffman tree
    * can be built in linear time with a queue
    */
    Node* extract(queue<Node*>& q, Nodes& ts) 
    {
    	Node* t;
    	if (!q.empty() && (ts.empty() || lessp(q.front(), ts.back()))) 
    	{
    		t = q.front();
    		q.pop();
    	}
    	else 
    	{
    		t = ts.back();
    		ts.pop_back();
    	}
    	return t;
    }
    
    Node* huffman2(Nodes ts) 
    {
    	queue<Node*> q;
    	sort(ts.begin(), ts.end(), greaterp);
    	Node* t = extract(q, ts);
    	while (!q.empty() || !ts.empty()) 
    	{
    		q.push(merge(t, extract(q, ts)));
    		t = extract(q, ts);
    	}
    	return t;
    }
    
    /* Build the code table from a Huffman tree by traversing */
    void codetab(Node* t, string bits, CodeTab& codes) 
    {
    	if (isleaf(t))
    		codes[t->c] = bits;
    	else 
    	{
    		codetab(t->left, bits + "0", codes);
    		codetab(t->right, bits + "1", codes);
    	}
    }
    
    CodeTab codetable(Node* t) 
    {
    	CodeTab codes;
    	codetab(t, "", codes);
    	return codes;
    }
    
    /* Encode text with the code table. */
    string encode(CodeTab codes, const char* w) 
    {
    	string bits;
    	while (*w)
    		bits += codes[*w++];
    	return bits;
    }
    
    /* Decode with a Huffman tree. */
    string decode(Node* root, const char* bits) 
    {
    	string w;
    	while (*bits) 
    	{
    		Node* t = root;
    		while (!isleaf(t))
    			t = '0' == *bits++ ? t->left : t->right;
    		w += t->c;
    	}
    	return w;
    }
    
    /*
    * Auxiliary function
    * Count the occurrence of every character to build the histogram of a text
    */
    map<char, int> freq(const char* w) 
    {
    	map<char, int> hist;
    	while (*w) ++hist[*w++];
    	return hist;
    }
    
    /* Turn a symbol-weight histogram into an array of huffman tree leaves. */
    Nodes nodes(const map<char, int>& hist) 
    {
    	vector<Node*> ns;
    	for (map<char, int>::const_iterator it = hist.begin(); it != hist.end(); ++it)
    		ns.push_back(leaf(it->first, it->second));
    	return ns;
    }
    
    void print_tr(Node* t, char end = '\n') 
    {
    	if (t) 
    	{
    		printf("(%c:%d ", t->c, t->w);
    		print_tr(t->left, 0);
    		print_tr(t->right, 0);
    		printf(")%c", end);
    	}
    }

```  
  

