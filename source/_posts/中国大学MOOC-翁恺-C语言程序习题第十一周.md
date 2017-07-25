---
title: 中国大学MOOC-翁恺-C语言程序习题第十一周
date: 2015-01-22 17:35:12
tags: C
category: PAT-C
---

#  11-0. 平面向量加法(10)

本题要求编写程序，计算两个二维平面向量的和向量。

** 输入格式： **

输入在一行中按照“x1 y1 x2 y2”的格式给出两个二维平面向量V1=(x1, y1)和V2=(x2, y2)的分量。

** 输出格式： **

在一行中按照“(x, y)”的格式输出和向量，坐标输出小数点后1位（注意不能输出-0.0）。
<!-- more -->
** 输入样例： **
    
    
    3.5 -2.7 -13.9 8.7
    

** 输出样例： **
    
    
    (-10.4, 6.0)
    

* * *

分析：这道题是一个求向量和的问题，先定义一个结构，然后x相加，y相加就完了咯，注意不能输出-0.0，判断一下就行了

    
```C
    #include<stdio.h>
    
    struct point { 
        double x;
        double y;
    };
     
    int main()
    {
        struct point v1;
        struct point v2;
        scanf("%lf %lf %lf %lf", &v1.x, &v1.y, &v2.x, &v2.y);
        struct point v;
        v = (struct point){v1.x + v2.x, v1.y + v2.y};
        if (v.x > -0.05 && v.x < 0.05)
            v.x = 0.0;
        if (v.y > -0.05 && v.y < 0.05)
            v.y = 0.0;    
     
        printf("(%.1lf, %.1lf)\n", v.x, v.y);
         
        return 0;
    }

```

#  11-1. 通讯录的录入与显示(10)

通讯录中的一条记录包含下述基本信息：朋友的姓名、出生日期、性别、固定电话号码、移动电话号码。本题要求编写程序，录入N条记录，并且根据要求显示任意某条记录。

** 输入格式： **

输入在第1行给出正整数N（<=10）；随后N行，每行按照格式“姓名 生日 性别 固话 手机”给出一条记录。其中“姓名”是不超过10个字符、不包含空格的非空字
符串；生日按“yyyy/mm/dd”的格式给出年月日；性别用“M”表示“男”、“F”表示“女”；“固话”和“手机”均为不超过15位的连续数字，前面有可能出现
“+”。

在通讯录记录输入完成后，最后一行给出正整数K，并且随后给出K个整数，表示要查询的记录编号（从0到N-1顺序编号）。数字间以空格分隔。

** 输出格式： **

对每一条要查询的记录编号，在一行中按照“姓名 固话 手机 性别 生日”的格式输出该记录。若要查询的记录不存在，则输出“Not Found”。

** 输入样例： **
    
    
    3
    Chris 1984/03/10 F +86181779452 13707010007
    LaoLao 1967/11/30 F 057187951100 +8618618623333
    QiaoLin 1980/01/01 M 84172333 10086
    2 1 7
    

** 输出样例： **
    
    
    LaoLao 057187951100 +8618618623333 F 1967/11/30
    Not Found
    

* * *

分析：这道题还是蛮简单的，构造一个结构体，我是直接定义了一个结构体，这样在输入的时候就可以方便很多，然后进行查找，找到输出，没找到则输出Not Found

```C
    
    #include<stdio.h>
    #include<stdbool.h>
    
    typedef struct information{
    	char name[11];
    	char birthday[11];
    	char sex;
    	char tel_line[17];
    	char mobile[17];
    }info;
    
    void getInfo(info* inf)
    {
    	scanf("%s %s %c %s %s",inf->name,inf->birthday,&inf->sex,inf->tel_line,inf->mobile);
    }
    
    void outInfo(info* inf)
    {
    	printf("%s %s %s %c %s\n",inf->name,inf->tel_line,inf->mobile,inf->sex,inf->birthday);
    }
    
    int main()
    {
    	info inf[10];
    	int N;
    	scanf("%d",&N);
    	int i;
    	for(i=0;i<N;i++)
    		getInfo(&inf[i]);
    	int k;
    	scanf("%d",&k);
    	int array[k];
    	for(i=0;i<k;i++)
    		scanf("%d",&array[i]);
    	for(i=0;i<k;i++)
    	{
    		if(array[i]>=0 && array[i]<N)
    			outInfo(&inf[array[i]]);
    		else
    			printf("Not Found\n");
    	}	
    	
    	return 0;
    }

```

  

==============================================================================
==========================

总结：最后一周也结束了，这些天通过这些pat基础题目把c语言基本语法过了遍，其实跟c++相似度蛮高的，哈哈。。。做的这些题目都蛮基础滴，也算巩固下自己的基础
啦。最后吐槽一下，这pat的格式问题就是坑，老是非得去用if去判断，有些地方真的不好弄的。。。 不过最后还是都解决了滴
![大笑](http://static.blog.csdn.net/xheditor/xheditor_emot/default/laugh.gif) ，后面准备去玩玩图形化编程，ACLLib好不好玩

呢，哈哈！！！  

  

  

