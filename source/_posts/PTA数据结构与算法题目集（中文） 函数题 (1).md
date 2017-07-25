
---
title: PTA数据结构与算法题目集（中文） 函数题 (1)
date: 2015-09-07 13:00:12
tags: 
  - 数据结构
  - 链表
category: PTA
---

##  4-1 单链表逆转

** code： **
    
```C++    
    List Reverse(List head)
    {
        if(NULL==head|| NULL==head->Next)
            return head;
        List p;
        List q;
        List r;
        p = head;
        q = head->Next;
        head->Next = NULL;
        while(q){
            r = q->Next;
            q->Next = p;
            p = q;
            q = r;
        }
        head=p;
        return head;
    }
```
<!-- more -->
_ P.S:就是一个反转链表，不是很难，耐心写就可以写完了 _

##  4-2 顺序表操作集

** code： **
    
```C++  
    Position Find( List L, ElementType X )
    {
        List r = L;
        while(r!=NULL && X!=r->Data)
        {
            r = r->Next;
        }
        if( r && X==r->Data)
            return r;
        else
            return ERROR;
    }
    List Insert( List L, ElementType X, Position P )
    {
        if(L==NULL)
        {
            if(P!=L) {
                printf("Wrong Position for Insertion");
                return ERROR;
            } else {
                L = (List)malloc(sizeof(struct LNode));
                L->Data = X;
                L->Next = NULL;
                return L;
            }
        } else {
            List r = L, s = NULL;
            while(r != P && r)
            {
                s = r;
                r = r->Next;
            }
            if(r==P)
            {
                List p = (List)malloc(sizeof(struct LNode));
                p->Data = X;
                if(s)
                {
                    p->Next = r;
                    s->Next = p;
                } else {
                    p->Next = L;
                    L = p;
                }
                    return L;
            } else {
                printf("Wrong Position for Insertion\n");
                return ERROR;
            }
        }
    }
    List Delete( List L, Position P )
    {
        List r = L;
        List pre = NULL;
        while(r && r!=P)
        {
            pre = r;
            r = r->Next;
        }
        if(r==P)
        {
            if(L == P)
            {
                L = L->Next;
                free(r);
            } else {
                pre->Next = P->Next;
                free(P);
                List k = pre->Next;
            }
            return L;
        }
        printf("Wrong Position for Deletion\n");
        return ERROR;
    }

```
_ P.S:这一题吧，做了蛮久的，也不是很难，但是有一个地方卡了很久，不过好像不记得哪了，囧~！总之，需要注意一些地方，看清楚题目中给的结点结构体。 _

##  4-3 求链式表的表长

** code **
```C++
    int Length( List L )
    {
        int len = 0;
        List p = L;
        while(p)
        {
            len++;
            p = p->Next;
        }
        return len;
    } 
```
** _ P.S:这道就相当简单了，一个函数求链表长度，将链表便利一遍，然后一个数加就行了，最后返回长度 _ **

##  4-4 链式表的按序号查找

** code: **
    
```C++  
    ElementType FindKth( List L, int K )
    {
        List p = L;
        if(K<1)
            return ERROR;
        int i=1;
        while(p && i<K)
        {
            p = p->Next;
            i++;
        }
        if(i==K && p!=NULL)
            return p->Data;
        else
            return ERROR;
    }
    
```
** _ P.S:这道函数题是需要写一个函数来找到并返回链式表的第K个元素，需要注意的就是这个第k个。 _ **

##  4-5 链式表操作集

** code: **
    
```C++ 
    Position Find( List L, ElementType X )
    {
        List r = L;
        while(r!=NULL && X!=r->Data)
        {
            r = r->Next;
        }
        if( r && X==r->Data)
            return r;
        else
            return ERROR;
    }
    List Insert( List L, ElementType X, Position P )
    {
        if(L==NULL)
        {
            if(P!=L) {
                printf("Wrong Position for Insertion");
                return ERROR;
            } else {
                L = (List)malloc(sizeof(struct LNode));
                L->Data = X;
                L->Next = NULL;
                return L;
            }
        } else {
            List r = L, s = NULL;
            while(r != P && r)
            {
                s = r;
                r = r->Next;
            }
            if(r==P)
            {
                List p = (List)malloc(sizeof(struct LNode));
                p->Data = X;
                if(s)
                {
                    p->Next = r;
                    s->Next = p;
                } else {
                    p->Next = L;
                    L = p;
                }
                    return L;
            } else {
                printf("Wrong Position for Insertion\n");
                return ERROR;
            }
        }
    }
    List Delete( List L, Position P )
    {
        List r = L;
        List pre = NULL;
        while(r && r!=P)
        {
            pre = r;
            r = r->Next;
        }
        if(r==P)
        {
            if(L == P)
            {
                L = L->Next;
                free(r);
            } else {
                pre->Next = P->Next;
                free(P);
                List k = pre->Next;
            }
            return L;
        }
        printf("Wrong Position for Deletion\n");
        return ERROR;
    }
```
** 这一题的代码的确开始粘过来的时候少了几行代码，现在已经更正了 **   
** —————————————————————– **

** _ P.S:我只想说这道题难过，一开始做的时候，按照本能的想法将Position当int整型数表示位置，可是很后来才发现它是一个指针类型，真的调了好久T_T，也就另一方面说明了一定要看清楚题目中的东西，链表的操作集合，相信学过数`据结构的应该都能写吧，自己慢慢写，总能通过的。这道题包括3个函数，找到X元素，指定位置插入元素函数，删除指定位置元素，需注意一下是哪一个。 _ **

