---
title: 中国大学MOOC-翁恺-C语言程序习题第七周
date: 2015-01-20 14:25:12
tags: C
category: PAT-C
---

#

#  07-0. 写出这个数 (20)

读入一个自然数n，计算其各位数字之和，用汉语拼音写出和的每一位数字。

** 输入格式： ** 每个测试输入包含1个测试用例，即给出自然数n的值。这里保证n小于10  100  。 

** 输出格式： ** 在一行内输出n的各位数字之和的每一位，拼音数字间有1空格，但一行中最后一个拼音数字后没有空格。 

** 输入样例： **
    
    
    1234567890987654321123456789
    

** 输出样例： **
    
    
    yi san wu
<!-- more -->    

* * *

  
分析：这道题和前面的05-2是一样的，就是这道要加一个求和的步骤，所以我也直接将前面的代码加了点就可以了

```C 
    
    #include<stdio.h>
    
    int main()
    {
    	char c;
    	int sum=0,count=1;
    	while(1)
    	{		
    		scanf("%c",&c);
    		if(c == '\n')
    			break;
    		sum += (int)c-48;
    	}
    	int m = sum;  
        while(m/10>0)           //求是几位数   
        {  
            count++;  
            m /= 10;   
        }  
    	int array[count];
    	int i,k;
    	for(i=count-1;i>=0;i--) 
    	{
    		k = sum%10;
    		array[i] = k;
    		sum /= 10;
    	}
    	for(i=0;i<count;i++)
    	{
    		if(i!=0)
    			printf(" "); 
    		switch(array[i])
    		{
    			case 0:
    				printf("ling");
    				break;
    			case 1:
    				printf("yi");
    				break;
    			case 2:
    				printf("er");
    				break;
    			case 3:
    				printf("san");
    				break;
    			case 4:
    				printf("si");
    				break;
    			case 5:
    				printf("wu");
    				break;
    			case 6:
    				printf("liu");
    				break;
    			case 7:
    				printf("qi");
    				break;
    			case 8:
    				printf("ba");
    				break;
    			case 9:
    				printf("jiu");
    				break;			
    		}
    	}
            return 0;
     } 

```

#  07-1. 换个格式输出整数 (15)

让我们用字母B来表示“百”、字母S表示“十”，用“12...n”来表示个位数字n（<10），换个格式来输出任一个不超过3位的正整数。例如234应该被输出为B
BSSS1234，因为它有2个“百”、3个“十”、以及个位的4。

** 输入格式： ** 每个测试输入包含1个测试用例，给出正整数n（<1000）。 

** 输出格式： ** 每个测试用例的输出占一行，用规定的格式输出n。 

** 输入样例1： **
    
    
    234
    

** 输出样例1： **
    
    
    BBSSS1234
    

** 输入样例2： **
    
    
    23
    

** 输出样例2： **
    
    
    SS123
    

* * *

分析：太简单了，不多解释

```C 
    
    #include<stdio.h>
    
    int main() 
    {
    	int N;
    	scanf("%d",&N);
    	int a = N/100;
    	int b = N/10%10;
    	int c = N%10;
    	int i,j,k;
    	for(i=1;i<=a;i++)
    		printf("B");
    	for(j=1;j<=b;j++)
    		printf("S");
    	for(k=1;k<=c;k++)
    		printf("%d",k);
    	
    	return 0;
    }

```

#  07-2. A+B和C (15)

给定区间[-2  31  , 2  31  ]内的3个整数A、B和C，请判断A+B是否大于C。

** 输入格式： **

输入第1行给出正整数T(<=10)，是测试用例的个数。随后给出T组测试用例，每组占一行，顺序给出A、B和C。整数间以空格分隔。

** 输出格式： **

对每组测试用例，在一行中输出“Case #X: true”如果A+B>C，否则输出“Case #X: false”，其中X是测试用例的编号（从1开始）。

** 输入样例： **
    
    
    4
    1 2 3
    2 3 4
    2147483647 0 2147483646
    0 -2147483648 -2147483647
    

** 输出样例： **
    
    
    Case #1: false
    Case #2: true
    Case #3: true
    Case #4: false
    

* * *

分析：这道题也就是一个输入一个判断一个的题目，不复杂

    
```C  
    #include<stdio.h>
    
    int main()
    {
    	int T;
    	scanf("%d",&T);
    	int i;
    	for(i=1;i<=T;i++)
    	{
    		long int A,B,C;
    		scanf("%ld %ld %ld",&A,&B,&C);
    		if(A+B>C)
    			printf("Case #%d: true\n",i);
    		else
    			printf("Case #%d: false\n",i);
    	}
    	
    	return 0;
    }

```

#  07-3. 数素数 (20)

令P  i  表示第i个素数。现任给两个正整数M <= N <= 10  4  ，请输出P  M  到P  N  的所有素数。

** 输入格式： **

输入在一行中给出M和N，其间以空格分隔。

** 输出格式： **

输出从P  M  到P  N  的所有素数，每10个数字占1行，其间以空格分隔，但行末不得有多余空格。

** 输入样例： **
    
    
    5 27
    

** 输出样例： **
    
    
    11 13 17 19 23 29 31 37 41 43
    47 53 59 61 67 71 73 79 83 89
    97 101 103
    

* * *

分析：这道题就是求素数，不难，但最坑的还是格式问题，也是pat最坑的地方，还要注意的就是不要超时，算法设计问题吧。这题还是蛮简单的

```C 
    
    #include<stdio.h>
    
    int isprime(int x)  
    {  
        int i,tmp;  
        if(x==2)  
            return 1;  
        if(x==0 || x==1)  
            return 0;  
        else  
        {  
            for(i=2;i*i<=x;i++)  
            {  
                if(x%i==0)  
                    return 0;  
            }  
        }  
        return 1;  
    } 
    
    int main()
    {
    	int M,N;
    	scanf("%d %d",&M,&N);
    	int i=2;
    	int count=0,flag=0;
    	while(count<=N)
    	{
    		if(isprime(i))
    		{
    			count++;
    			if(count>=M && count<=N)
    			{
    				flag++;
    				if(flag%10==0)
    				printf("%d\n",i);	
    				else if(count==N)
    					printf("%d",i);
    				else
    					printf("%d ",i);				
    			}
    		}
    		i++;				
    	}
    	
    	return 0;
    }
```
  

总结：第七周的结束了，这几道题难度都不打，算法也都是很简单的，总之还是注意下输出格式就好  

