---
title: PTA数据结构 5-3 树的同构
date: 2015-10-09 14:12:12
tags: 
  - 数据结构
  - Tree
category: PTA
---

** 题目： **   
给定两棵树T1和T2。如果T1可以通过若干次左右孩子互换就变成T2，则我们称两棵树是“同构”的。例如图1给出的两棵树就是同构的，因为我们把其中一棵树的结点A、B、G的左右孩子互换后，就得到另外一棵树。而图2就不是同构的。  
<!-- more -->
![图1](http://pta.patest.cn/api/image/attachment?id=28)  
图1  
![图2](http://pta.patest.cn/api/image/attachment?id=29)  
图2

现给定两棵树，请你判断它们是否是同构的。  
输入格式:  
输入给出2棵二叉树树的信息。对于每棵树，首先在一行中给出一个非负整数N (≤10)，即该树的结点数（此时假设结点从0到N−1编号）；随后N行，第i行对应编号
第i个结点，给出该结点中存储的1个英文大写字母、其左孩子结点的编号、右孩子结点的编号。如果孩子结点为空，则在相应位置上给出“-”。给出的数据间用一个空格分隔
。注意：题目保证每个结点中存储的字母是不同的。

输出格式:  
如果两棵树是同构的，输出“Yes”，否则输出“No”。  
输入样例1（对应图1）：

    
    
    8
    A 1 2
    B 3 4
    C 5 -
    D - -
    E 6 -
    G 7 -
    F - -
    H - -
    8
    G - 4
    B 7 6
    F - -
    A 5 1
    H - -
    C 0 -
    D - -
    E 2 -

输出样例1:

    
    
    Yes

输入样例2（对应图2）：

    
    
    8
    B 5 7
    F - -
    A 0 3
    C 6 -
    H - -
    D - -
    G 4 -
    E 1 -
    8
    D 6 -
    B 5 -
    E - -
    H - -
    C 0 2
    G - 3
    F - -
    A 1 4

输出样例2:

    
    
    No

* * *

** 分析与思路：  **   
&emsp;&emsp;此题是给出了两个树，来判断这两棵树是否属于同构，而何为同构呢？根据题中的意思，我们可以知道，两棵树中包含的结点个数和元素必须相同的，而对于第一棵树的每一个结点呢，在第一棵树我们都能找到一个与之对应的结点，并且它们的左右孩子结点的元素是相同的话，当然左右可以互换，这样的两棵树就是同构的。  
&emsp;&emsp;通过上面的解释我们就可以有这样一个思路，首先肯定是通过题目的输入来构造出两棵树，然后我们就对第一棵树中每一个结点来找出第二棵树中的对应结点，然后判断它们的孩子结点元素是否是一样的，这就是按照题目的意思来实现。这就是方法一。  
&emsp;&emsp;而我呢自己做题时却产生了一种另外的想法，既然每个结点需要判断它们的孩子结点的元素是否一样，我们需要分不少的情况来考虑，还有空的情况，所以很繁琐。而我们换一种思路，我们以它们的孩子结点为基准，来判断它们的父亲结点，是不是更好呢？这样我们就完全不用考虑左右孩子的那么多种情况了，只需要想如果两个结点他们的元素相同，那么它们的父亲结点是否是一样的元素，如果不是，那么这两棵树必然不符合同构，这就是我自己想的方法二。  
下面给出两种方法的代码，都通过了测试。第一种是采用了ｍｏｏｃ里的代码，没怎么改，第二种自己实现了。

* * *

** method_1 code： **
```C++    
    
    #include <iostream>
    using namespace std;
    
    #define MaxTree 10 
    
    typedef char ElementType;
    typedef int Tree;
    struct TreeNode 
    { 
        ElementType Data;  
        Tree  Left;  
        Tree  Right; 
    } T1[MaxTree], T2[MaxTree];
    
    Tree BuildTree(struct TreeNode T[]); 
    bool  Isomorphic(Tree R1, Tree R2);
    int main()
    {
        Tree R1, R2;
        R1 = BuildTree(T1);       
        R2 = BuildTree(T2);       
        if (Isomorphic(R1, R2))  
            printf("Yes\n");    
        else  
            printf("No\n");        
    
        return 0;
    }
    
    Tree BuildTree(struct TreeNode T[])
    {
        int N;
        Tree Root;      // 根结点
        cin>>N;  
        if (N) {
            int *check = new int[N];
            for (int i = 0; i < N; i++)
                check[i] = 0;
            for (int i = 0; i < N; i++) {
                char c_left, c_right;
                cin >> T[i].Data >> c_left >> c_right;
                if (c_left != '-') {
                    T[i].Left = c_left - '0';
                    check[T[i].Left] = 1;
                }
                else {
                    T[i].Left = -1;
                }
                if (c_right != '-') {
                    T[i].Right = c_right - '0';
                    check[T[i].Right] = 1;
                }
                else {
                    T[i].Right = -1;
                }
            }
            int i;
            for (i = 0; i < N; i++) {
                if (!check[i])
                    break;
            }
            Root = i;
        }
        else
            Root = -1;
        return Root;
    }
    
    bool  Isomorphic(Tree R1, Tree R2) 
    { 
        /* both empty */  
        if ((R1 == -1) && (R2 == -1))
            return  true;        
        /* one of them is empty */
        if (((R1 == -1) && (R2 != -1)) || ((R1 != -1) && (R2 == -1)))   
            return  false;          
        /* roots are different */  
        if (T1[R1].Data != T2[R2].Data)   
            return  false;        
        /* both have no left subtree */   
        if ((T1[R1].Left == -1) && (T2[R2].Left == -1))   
            return  Isomorphic(T1[R1].Right, T2[R2].Right);  
        /* no need to swap the left and the right */       
        if (((T1[R1].Left != -1) && (T2[R2].Left != -1)) 
            && ((T1[T1[R1].Left].Data) == (T2[T2[R2].Left].Data)))                   
            return  (Isomorphic(T1[R1].Left, T2[R2].Left) && Isomorphic(T1[R1].Right, T2[R2].Right));    
        /* need to swap the left and the right  */                    
        else      
            return (Isomorphic(T1[R1].Left, T2[R2].Right) && Isomorphic(T1[R1].Right, T2[R2].Left));
    }
```
** method_2 code: **
    
```C++
    #include <iostream>
    #include <vector>
    using namespace std;
    
    typedef char ElementType;
    typedef struct TreeNode* BinTree;
    struct TreeNode {
        ElementType Data;
        BinTree Left = NULL;
        BinTree Right = NULL;
        BinTree Parent = NULL;
    };
    
    vector<TreeNode> create_vector(int N);
    bool isomorphic(vector<TreeNode> v1, vector<TreeNode> v2);
    int main()
    {
        int N;
        cin >> N;
        vector<TreeNode> v1 = create_vector(N);
        cin >> N;
        vector<TreeNode> v2 = create_vector(N);
        if (isomorphic(v1, v2))
            cout << "Yes" << endl;
        else
            cout << "No" << endl;
        return 0;
    }
    
    vector<TreeNode> create_vector(int N)
    {
        // 创建一个数组来存放各个结点
        vector<TreeNode> vec(N);
        char data;
        char left, right;
        // 初始化
        for (int i = 0; i < N; i++) {
            TreeNode *t_node = new TreeNode;
            cin >> data >> left >> right;
            t_node->Data = data;
            /* 对输入进行处理并存储 */
            if (left != '-') {
                t_node->Left = &vec[(left - '0')];
                vec[(left - '0')].Parent = t_node;
            }
            if (right != '-') {
                t_node->Right = &vec[(right - '0')];
                vec[(right - '0')].Parent = t_node;
            }
            vec[i].Data = t_node->Data;
            vec[i].Left = t_node->Left;
            vec[i].Right = t_node->Right;
        }
        return vec;
    }
    
    /* 对每个vec中的元素分析，通过判断它们的父节点数据是否一样来判断它们是否同构 */
    bool isomorphic(vector<TreeNode> v1, vector<TreeNode> v2)
    {
        /* 如果两棵树结点个数不一样，肯定错误 */
        if (v1.size() != v2.size())
            return false;
        bool flag = false;
        for (int i = 0; i < v1.size(); i++) {
            for (int j = 0; j < v2.size(); j++) {
                /* 当找到元素相同的结点时 */
                if (v1[i].Data == v2[j].Data) {
                    flag = true;
                    /* 两个节点的父亲结点均不为空 */
                    if (v1[i].Parent && v2[j].Parent) {
                        /* 父结点的元素也相同时，说明找到了 */
                        if (v1[i].Parent->Data != v2[j].Parent->Data) {
                            return false;
                        }
                    }
                    /* 当两个元素结点中存在没有没有父结点的结点时 */
                    else {
                        /* 如果都为空，说明是相同的 */
                        if (v1[i].Parent == NULL && v2[j].Parent == NULL) {
                            flag = true;
                            break;
                        }
                        /* 其他情况都是表示两个结点元素相同，但父结点元素不同，必然不是同构 */
                        else {
                            return false;
                        }
                    }
                }
                /* 此时还需判断一下经过一次遍历，v2中是否找到了与v1结点元素相同结点，若没有则必然不是同构*/
                if (j == v2.size() - 1 && !flag)
                    return false;
            }
        }
        return true;
    }
```
* * *

