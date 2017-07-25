---
title: Reversing Linked List
date: 2014-12-26 15:02:12
tags: 
  - 数据结构
  - 链表
category: PAT
---

&emsp;&emsp;Given a constant K and a singly linked list L, you are supposed to reverse the links of every K elements on L. For example, given L being 1→2→3→4→5→6, if K = 3, then you must output 3→2→1→6→5→4; if K = 4, you must output 4→3→2→1→5→6.
<!-- more -->
** Input Specification: **

&emsp;&emsp;Each input file contains one test case. For each case, the first line contains the address of the first node, a positive N (<= 10  5  ) which is the total number of nodes, and a positive K (<=N) which is the length of the sublist to be reversed. The address of a node is a 5-digit nonnegative integer, and NULL is represented by -1.

&emsp;&emsp;Then N lines follow, each describes a node in the format:

_ Address Data Next _

where _ Address _ is the position of the node, _ Data _ is an integer, and _ Next _ is the position of the next node.

** Output Specification: **

For each case, output the resulting ordered linked list. Each node occupies a line, and is printed in the same format as in the input.

** Sample Input: **
 
    
    00100 6 4
    00000 4 99999
    00100 1 12309
    68237 6 -1
    33218 3 00000
    99999 5 68237
    12309 2 33218
    

** Sample Output: **
    
    
    00000 4 33218
    33218 3 12309
    12309 2 00100
    00100 1 99999
    99999 5 68237
    68237 6 -1

  

** 奋战了好久，终于通过测试了，泪奔。。。 **
    
```C++
    #include<iostream>
    #include<iomanip>
    #include<deque>
    using namespace std;
    
    #define FORMAT setw(5) << setfill('0')
    struct node
    {
    	int data;
    	int address;
    	int next;
    }an[1000000];
    
    deque<node> ans;
    int flag = 0;
    
    void output(deque<node> ans)
    {
    	if (flag>0 && !ans.empty())
    	{
    		cout << FORMAT << ans.back().address << endl;
    		flag--;
    	}
    	int index = ans.size() - 2;
    	int temp = ans.back().next;
    	while (!ans.empty())
    	{
    		if (index == -1)
    		{
    			cout << FORMAT << ans.back().address << " ";
    			cout << ans.back().data << " ";
    			flag++;
    		}
    		else
    		{
    			cout << FORMAT << ans.back().address << " ";
    			cout << ans.back().data << " ";
    			if (ans.at(index).address == -1)
    				cout << ans.at(index).address << endl;
    			else
    				cout << FORMAT << ans.at(index).address << endl;
    		}
    		ans.pop_back();
    		index -= 1;
    	}
    }
    
    void output_front(deque<node> &ans)
    {
    	if (flag>0 && !ans.empty())
    	{
    		cout << FORMAT << ans.front().address << endl;
    		flag--;
    	}
    	while (!ans.empty())
    	{
    		cout << FORMAT << ans.front().address << " ";
    		cout << ans.front().data << " ";
    		if (ans.back().next == -1)
    		{
    			cout << ans.front().next << endl;
    		}
    		else
    			cout << FORMAT << ans.front().next << endl;
    		ans.pop_front();
    	}
    }
    
    
    int main()
    {
    	int n ,k , head;
    	cin >> head >> n >> k;
    	int addr, data, next;
    	for (int i = 0; i<n; i++)
    	{
    		cin >> addr >> data >> next;
    		an[addr].address = addr;
    		an[addr].data = data;
    		an[addr].next = next;
    	}
    	int cnt = 0;
    	int index = head;
    	bool apend = false;
    	while (cnt<n)
    	{
    		next = an[index].next;
    		node tempn;
    		tempn.address = an[index].address;
    		tempn.data = an[index].data;
    		tempn.next = next;
    		ans.push_back(tempn);
    		cnt++;
    		if (cnt%k == 0)
    		{
    			output(ans);
    			ans.clear();
    		}
    		if (next == -1)
    		{
    			if (ans.empty())
    			{
    				apend = true;
    			}
    			output_front(ans);
    			if (apend)
    			{
    				cout << "-1" << endl;
    			}
    			break;
    		}
    		index = next;
    	}
    	return 0;
    }
```
