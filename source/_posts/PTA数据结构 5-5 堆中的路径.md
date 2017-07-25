---
title: PTA数据结构 5-5 堆中的路径
date: 2015-10-09 20:42:24
tags: 
  - 数据结构
  - 堆
category: PTA
---

** 题目：  **

&emsp;&emsp;将一系列给定数字插入一个初始为空的小顶堆H[]。随后对任意给定的下标i，打印从H[i]到根结点的路径。  
** 输入格式: **   
&emsp;&emsp;每组测试第1行包含2个正整数N和M(≤1000)，分别是插入元素的个数、以及需要打印的路径条数。下一行给出区间[-10000,10000]内的N个要被插入一个初始为空的小顶堆的整数。最后一行给出M个下标。  
** 输出格式: **   
&emsp;&emsp;对输入中给出的每个下标i，在一行中输出从H[i]到根结点的路径上的数据。数字间以1个空格分隔，行末不得有多余空格。
<!-- more -->
** 输入样例: **
    
    
    5 3
    46 23 26 24 10
    5 4 3

** 输出样例: **
    
    
    24 23 10
    46 23 10
    26 10

** 分析与思路： **   
&emsp;&emsp;此题自己开始写了，也是建堆，插入，再输出，弄了半天，十分烦躁，然后就开始去Internet寻找些呀，看看有没有什么好的呢，到mooc时发现小白专场又有这道题哈，然后就看视频看了一遍，我好想吐槽呀，我咋就这么蠢嘞，用数组可以了呀，还傻逼样的去建堆，因此有些时候不要看到树，堆得时候应该先想想是否可以不用建树，建堆，而不是盲目的立刻就写，有时候采用数组就可以更好地解决，这题用数组存储数据多好呀。创建堆，插入都是很简单的。 [ [链接] ](http://www.icourse163.org/learn/zju-93001?tid=360003#/learn/content?type=detail&id=718031&cid=797008)

** code: **
    
```C++    
    #include <iostream>
    using namespace std;
    
    #define MAXN 1001
    #define MINH -10001
    
    void Create();
    void Insert(int x);
    int H[MAXN], size;      // 全局变量， 堆的数组和大小
    int main()
    {
        int N, M, x, j;
        cin>>N>>M;      /* 输入第一行的两个数 */
        Create();       /* 堆初始化 */
        for(int i=0; i<N; i++) {    /*以逐个插入方式建堆 */
            cin>>x;
            Insert(x);
        }
        for(int k=0; k<M; k++) {
            cin>>j;
            cout<<H[j];
            while (j>1) { /*沿根方向输出各结点*/
                j /= 2;
                cout<<" "<<H[j];     /*输出结点*/
            }
            cout<<endl;
        }
        return 0;
    }
    
    void Create()
    {
        size = 0;
        H[0] = MINH;
    }
    
    void Insert(int x)
    {
        if(size==1000) // 堆已满
            return;
        int i;
        size++;     // 堆大小加一
        for (i = size; H[i/2] > x; i/=2) {
            H[i] = H[i/2];
        }
        H[i] = x;
    }
    
```