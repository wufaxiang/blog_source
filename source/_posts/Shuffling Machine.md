---
title: 自测5. Shuffling Machine
date: 2015-02-27 20:13:12
tags: 
  - 数据结构
  - shuffling
category: PAT
---

&emsp;&emsp;Shuffling is a procedure used to randomize a deck of playing cards. Because standard shuffling techniques are seen as weak, and in order to avoid "inside jobs" where employees collaborate with gamblers by performing inadequate shuffles, many casinos employ ** automatic shuffling machines ** . Your task is to simulate a shuffling machine.

&emsp;&emsp;The machine shuffles a deck of 54 cards according to a given random order and repeats for a given number of times. It is assumed that the initial status of a card deck is in the following order:

	S1, S2, ..., S13, H1, H2, ..., H13, C1, C2, ..., C13, D1, D2, ..., D13, J1, J2
<!-- more -->
&emsp;&emsp;where "S" stands for "Spade", "H" for "Heart", "C" for "Club", "D" for "Diamond", and "J" for "Joker". A given order is a permutation of distinct integers in [1, 54]. If the number at the i-th position is j, it means to move the card from position i to position j. For example, suppose we only have 5 cards: S3, H5, C1, D13 and J2. Given a shuffling order {4, 2, 5, 3, 1}, the result will be: J2, H5, D13, S3, C1. If we are to repeat the shuffling again, the result will be: C1, H5, S3, J2, D13.

** Input Specification: **

&emsp;&emsp;Each input file contains one test case. For each case, the first line contains a positive integer K (<= 20) which is the number of repeat times. Then the next line contains the given order. All the numbers in a line are separated by a space.

** Output Specification: **

&emsp;&emsp;For each test case, print the shuffling results in one line. All the cards are separated by a space, and there must be no extra space at the end of the line.

** Sample Input: **
    
    
    2
    36 52 37 38 3 39 40 53 54 41 11 12 13 42 43 44 2 4 23 24 25 26 27 6 7 8 48 49 50 51 9 10 14 15 16 5 17 18 19 1 20 21 22 28 29 30 31 32 33 34 35 45 46 47
    

** Sample Output: **
    
    
    S7 C11 C10 C12 S1 H7 H8 H9 D8 D9 S11 S12 S13 D10 D11 D12 S3 S4 S6 S10 H1 H2 C13 D2 D3 D4 H6 H3 D13 J1 J2 C1 C2 C3 C4 D1 S5 H5 H11 H12 C6 C7 C8 C9 S2 S8 S9 H10 D5 D6 D7 H4 H13 C5
    

* * *

&emsp;&emsp;分析：先吐槽一下，看完这道题真累，不过等到看完看懂之后，表示看什么题目嘛，直接看那个栗子不就好了（当时还在拿着手机查单词的说，英语渣渣就是难过），这道题别看
这么多有点吓人，其实看懂之后你会发现，这道题的思路很清晰，算法也就一条语句，所以看懂题目，特别是那个栗子，你就能做出来哒，想说下思路，可是感觉没什么好说的，
题目都告诉你了，就那个栗子，剩下的就是自己定义数组存储的问题了，如果还不清楚的直接看代码好啦...

code:

```C++
    #include <stdio.h>
    #include <stdlib.h>
    
    typedef struct
    {
        char type;
        int num;
    }card;
    int main()
    {
        int reapeat_time;
        int shuffle_order[55];      //洗牌顺序
        card card_order[55];        //卡片顺序
        card card_order_2[55];
        scanf("%d",&reapeat_time);  //输入重复次数
        int i;
        //存储卡片顺序
        for(i=1;i<55;i++)
        {
            if(i<14)
            {
                card_order[i].type = 'S';
                card_order[i].num = i;
            }
            else if(i<27)
            {
                card_order[i].type = 'H';
                card_order[i].num = i-13;
            }
            else if(i<40)
            {
                card_order[i].type = 'C';
                card_order[i].num = i-26;
            }
            else if(i<53)
            {
                card_order[i].type = 'D';
                card_order[i].num = i-39;
            }
            else
            {
                card_order[i].type = 'J';
                card_order[i].num = i-52;
            }
        }
    
        int number;
        for(i=1;i<55;i++)       //输入shuff顺序
        {
            scanf("%d",&number);
            shuffle_order[i] = number;  //存储到数组中
        }
        int s=1;
        while(s<=reapeat_time)
        {
            for(i=1;i<55;i++)
            {
                card_order_2[shuffle_order[i]] = card_order[i];
            }
            for(i=1;i<55;i++)
            {
                card_order[i] =  card_order_2[i];
            }
            s++;
        }
        for(i=1;i<55;i++)
        {
            printf("%c%d",card_order[i].type,card_order[i].num);
            if(i!=54)
                printf(" ");
        }
        return 0;
    }
    

```

