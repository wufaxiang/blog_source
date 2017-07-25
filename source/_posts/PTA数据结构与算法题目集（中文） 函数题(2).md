---
title: PTA数据结构与算法题目集（中文） 函数题(2)
date: 2015-09-07 13:00:24
tags: 
  - 数据结构
  - 链表
category: PTA
---

##  4-6 带头结点的链式表操作集

code:

    
```C++   
    List MakeEmpty()
    {
        List L;
        L = (List)malloc(sizeof(struct LNode));
        L->Data = NULL;
        L->Next = NULL;
        return L;
    }
    Position Find( List L, ElementType X )
    {
        List p = L;
        while(p && p->Data!=X) {
            p = p->Next;
        }
        if(p && X==p->Data) {
            return p;
        } else {
            return ERROR;
        }
    }
<!-- more -->
    bool Insert( List L, ElementType X, Position P )
    {
        List r = L;
        List pre = L;
        while(r && P!=r) {
            pre = r;
            r = r->Next;
        }
        if(r==P) {
            List p = (List)malloc(sizeof(struct LNode));
            p->Data = X;
            p->Next = r;
            pre->Next = p;
            return true;
        }
        printf("Wrong Position for Insertion\n");
        return false;
    }
    bool Delete( List L, Position P )
    {
    
        List r = L;
        List pre = L;
        while(r && r!=P)
        {
            pre = r;
            r = r->Next;
        }
        if(r==P) {
            pre->Next = P->Next;
            free(P);
            List k = pre->Next;
            return true;
        }
        printf("Wrong Position for Deletion\n");
        return false;
    }
```
** _ P.S:这道题需要写四个函数，分别是链表的置空，查询某个元素，还有元素的插入和删除，都是一些链表的基本操作，题中的链表是有头节点的，而开始的题中的链表是没有使用头节点的，这个与前面的有一些区别，其实拥有头节点的链表更容易操作，在插入时不用考虑是否是第一个结点了，更加好写些，同样要注意Find函数，它需要返回的是这个元素的Position，而它是一个指针变量，而不是代表其位置的整型变量。 _ **

##  4-7 在一个数组中实现两个堆栈

** _ P.S:很头疼，这一题没有通过，报的错误也是十分无奈，超时，所以也不知道怎样去解决这个问题，如何去优化代码，这道题其实是一个数组嘛，两个栈的栈底在两头，设计应该就是这样，但是给我报超时错误，暂时先放下吧，以后通过再补上来。 _ **

** _ 更新：补上成功的code，今天突然再次提交，居然就通过了，看来我的想法是没有问题的^_^ _ **

** code: **
    
```C++
    Stack CreateStack( int MaxSize )
    {
        Stack S = (Stack)malloc(sizeof(struct SNode));
        S->MaxSize = MaxSize;
        S->Data = (ElementType *)malloc(sizeof(ElementType)*MaxSize);
        S->Top2 = MaxSize;
        S->Top1 = -1;
        return S;
    }
    bool Push(Stack S, ElementType X, int Tag){
        if(S->Top1+1 == S->Top2){
            printf("Stack Full\n");
            return false;
        }else{
            if(Tag == 1){
                S->Top1++;
                S->Data[S->Top1] = X;
            }else{
                S->Top2--;
                S->Data[S->Top2] = X;
            }
        }
        return true;
    }
    
    ElementType Pop(Stack S, int Tag){
        int X;
        if(Tag == 1){
            if(S->Top1 == -1){
                printf("Stack 1 Empty\n");
                return ERROR;
            }else{
                X = S->Data[S->Top1];
                S->Top1--;
                return X;
            }
        }else if(Tag == 2){
            if(S->Top2 == S->MaxSize){
                printf("Stack 2 Empty\n");
                return ERROR;
            }else{
                X = S->Data[S->Top2];
                S->Top2++;
                return X;
            }
        }
    }
    

##  4-8 求二叉树高度

** code: **
    
    
    int GetHeight( BinTree BT )
    {
        if (BT == NULL)  
            return 0;  
        else {  
            int left = GetHeight(BT->Left);
            int right = GetHeight(BT->Right);
            if(left>=right)
                return 1 + left;
            else 
                return 1 + right;
        }  
    }
```
** _ P.S:求一颗二叉树的高度，我直接使用了递归实现，所以也十分的清晰易懂。 _ **

##  4-9 二叉树遍历

** _ 这道题吧，也木有通过，其实是不愿写吧，前序，中序，后续遍历都很简单，但是层序便利时，我们知道需要借助队列来实现，而在函数题中又不能用那种头文件，因此我想了想，但后来脑子就不舒服，先放下，后面补上。 _ **

** _ 更新：今天想了想，其实没有必要用队列的，可以简单点嘛，开始怎么就那么蠢呢，队列也是由数组或链表构成的，而这道题又不需要考虑队列满或者空的情况，自己用一个数组代替不就是了，其实很简单的啦，我才用了一个容量为100的数组来模拟队列，最后就这样成功啦，前中后三种遍历就没有什么好说的咯，递归，简单易懂，用栈的话也不难，但是能简单就简单嘛 _ **

** code: **
    
```C++
    void InorderTraversal( BinTree BT ){
        if(BT){
            InorderTraversal(BT->Left);
            printf(" %c", BT->Data);
            InorderTraversal(BT->Right);
        }
    }
    
    void PreorderTraversal( BinTree BT ){
        if(BT){
            printf(" %c",BT->Data);
            PreorderTraversal(BT->Left);
            PreorderTraversal(BT->Right);
        }
    }
    
    void PostorderTraversal( BinTree BT )
    {
        if(BT != NULL){
            PostorderTraversal(BT->Left);
            PostorderTraversal(BT->Right);
            printf(" %c", BT->Data);
        }
    }
    
    void LevelorderTraversal( BinTree BT ) {
        BinTree str[100];
        int rear = -1, front = -1;
        BinTree T;
    
        if(!BT)
            return;
        rear = rear+1;
        str[rear] = BT;
        while(rear != front){
            front = front+1;
            T = str[front];
            printf(" %c", T->Data);
            if(T->Left) {
                rear = rear+1;
                str[rear] = T->Left;
            }
            if(T->Right){
                rear++ ;
                str[rear] = T->Right;
            }
        }
    }
```
##  4-10 二分查找

** code: **
    
```C++
    Position BinarySearch( List Tbl, ElementType K )
    {
        int left, right, middle;
        left = 1, right = Tbl->Last;
        while (left <= right)
        {
            middle = (left + right) / 2;
            if (Tbl->Data[middle] > K)
            {
                right = middle - 1;
            }
            else if (Tbl->Data[middle] < K)
            {
                left = middle + 1;
            }
            else if(Tbl->Data[middle] == K)
            {
                return middle;
            }
        }
        return NotFound;
    }
```
** _ P.S:二分查找算法，也叫折半查找，也就是将数组每次只查找一半，因此可以省去很多的比较次数，也就提高了效率，减少了查找时间，是一个很好，很基础的算法 _ **   
* 

##  4-11 先序输出叶结点

** code: **
    
```C++
    void PreorderPrintLeaves( BinTree BT ){
        if(BT != NULL){
            PreorderPrintLeaves(BT->Left);
            if(BT->Data >='A' && BT->Data <='z' && BT->Left == NULL && BT->Right == NULL){
                printf(" %c",BT->Data);
            }
            PreorderPrintLeaves(BT->Right);
        }
    }
```
** _ P.S:先序输出叶子结点，说白了就是先序遍历呗，不过在先序遍历的基础上进行一下判断，当左右子节点都为空时才输出来。 _ **

##  4-12 二叉搜索树的操作集

** _ P.S:这道题部分正确，可能是没有考虑全面，前三个测试点不能通过，后两个可以，所以等通过之后再放上代码。 _ **

** _ 更新：调了很多次，这道虽然是教材上的，但是删除操作是真的很复杂，需要很细心头脑很清晰的去写，下面的是code，参考了教材上的源码，最后居然让我发现了一个小错误，教材上查找最大最小元素函数的判断语句中多了个“！”，很容易就发现啦，但是不一定会注意到，慢慢写啦！！ _ **

** code: **
    
```C++
    BinTree Insert( BinTree BST, ElementType X )
    {
        if(!BST) {          /* 若原树为空，生成并返回一个结点的二叉搜索树 */
            BST = (BinTree)malloc(sizeof(struct TNode));
            BST ->Data = X;
            BST ->Left = BST ->Right = NULL;
        }else {         /* 开始寻找要插入元素的位置 */
            if(X < BST ->Data ) {
                BST ->Left = Insert(BST ->Left, X);
            }else if(X > BST ->Data ) {
                BST ->Right = Insert(BST ->Right, X);
            }
            /* X已经存在，不用操作 */
        }
        return BST;
    }
    BinTree Delete( BinTree BST, ElementType X )
    {
        Position Tmp;
        if(!BST) {
            printf("Not Found\n");
        }else {
            if( X < BST->Data) {
                BST ->Left = Delete(BST->Left, X);  /* 左子树递归删除 */
            }else if(X > BST->Data ) {
                BST ->Right = Delete(BST->Right , X);    /* 右子树递归删除*/
            }else {     /* 找到需要删除的结点 */
                if(BST->Left && BST->Right) {
                    /* 被删除的结点有左右子结点 */
                    Tmp = FindMin( BST ->Right);   /* 在右子树中找到最小结点填充删除结点 */
                    BST ->Data = Tmp ->Data;
                    BST ->Right = Delete(BST ->Right, BST ->Data);  /* 在删除结点的右结点中删除最小元素 */
                }else {     /* 被删除结点有一个或没有子结点*/
                    Tmp = BST;
                    if(!BST ->Left) {   /*有右孩子或无子结点*/
                        BST = BST->Right;
                    }else if(!BST->Right) {  /* 有左孩子或无子节点*/
                        BST = BST->Left;
                    }
                    free(Tmp);
                }
            }
        }
        return BST;
    }
    Position Find( BinTree BST, ElementType X )
    {
        if(!BST) {
            return NULL;    // 查找失败
        }
        if(X > BST->Data) {
            return Find(BST->Right, X);     //在右子树中继续查找
        }else if(X < BST->Data) {
            return Find(BST->Left, X);      //在左子树中继续查找
        }else {
            return BST;                     //查找成功
        }
    }
    Position FindMin( BinTree BST )
    {
        if(BST) {
            while(BST->Left) {
                BST = BST->Left;        //沿着左分支一直查找，直到叶子结点
            }
        }
        return BST;
    }
    Position FindMax( BinTree BST )
    {
        if(BST) {
            while(BST->Right) {
                BST = BST->Right;        //沿着右分支一直查找，直到叶子结点
            }
        }
        return BST;
    }
```
** 函数题就这12道，其中已经完全通过了9道，还有3道暂时没有通过，要不超时，要不部分正确，暂时想也想不出来，想的也比较难过，所以等什么时候有空无聊时再想想，然后通过了再补上吧，如果能给予想法的话请联系我呀，一起交流，一起分享，一起进步^_^ **

** 今天将原先没放上的都解决啦，12道函数题完成，虽然都是很基础的数据结构，但是真要全部靠自己写出来很困难困难，多多练习咯，后面就慢慢刷编程题咯，刷题的快感~~~ **

