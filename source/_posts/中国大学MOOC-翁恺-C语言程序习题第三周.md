---
title: 中国大学MOOC-翁恺-C语言程序习题第三周
date: 2015-01-19 12:16:12
tags: C
category: PAT-C
---

#

#  03-0. 超速判断(10)

模拟交通警察的雷达测速仪。输入汽车速度，如果速度超出60 mph，则显示“Speeding”，否则显示“OK”。

** 输入格式： **

输入在一行中给出1个不超过500的非负整数，即雷达测到的车速。

** 输出格式： **

在一行中输出测速仪显示结果，格式为：“Speed: V - S”，其中V是车速，S或者是Speeding、或者是OK。
<!-- more -->
** 输入样例1： **
    
    
    40
    

** 输出样例1： **
    
    
    Speed: 40 - OK
    

** 输入样例2： **
    
    
    75
    

** 输出样例2： **
    
    
    Speed: 75 - Speeding
    

* * *

  

```C 
    
    #include<stdio.h>
    
    int main()
    {
    	int speed;
    	scanf("%d",&speed);
    	printf("Speed: %d - ", speed);
    	if(speed<=60)
    		printf("OK");
    	else
    		printf("Speeding");
    } 

```  

#  03-1. 三天打鱼两天晒网(15)

中国有句俗语叫“三天打鱼两天晒网”。假设某人从某天起，开始“三天打鱼两天晒网”，问这个人在以后的第N天中是“打鱼”还是“晒网”？

** 输入格式： **

输入在一行中给出1个不超过1000的正整数N。

** 输出格式： **

在一行中输出此人在第N天中是“Fishing”（即“打鱼”）还是“Drying”（即“晒网”），并且输出“in day N”。

** 输入样例1： **
    
    
    103
    

** 输出样例1： **
    
    
    Fishing in day 103
    

** 输入样例2： **
    
    
    34
    

** 输出样例2： **
    
    
    Drying in day 34
    

* * *

  

```C  
    
    #include<stdio.h>
    
    int main()
    {
    	int day;
    	scanf("%d",&day);
    	int k = day%5;
    	if(k==4 || k==0)
    		printf("Drying ");		
    	else
    		printf("Fishing ");
    	printf("in day %d" ,day);
    	return 0;
    }

```

#  03-2. 用天平找小球(10)

三个球A、B、C，大小形状相同且其中有一个球与其他球重量不同。要求找出这个不一样的球。

** 输入格式： **

输入在一行中给出3个正整数，顺序对应球A、B、C的重量。

** 输出格式： **

在一行中输出唯一的那个不一样的球。

** 输入样例： **
    
    
    1 1 2
    

** 输出样例： **
    
    
    C
    

* * *
    
```C
    #include<stdio.h>
    
    int main()
    {
    	int a,b,c;
    	scanf("%d %d %d",&a,&b,&c);
    	if(a==b)
    		printf("C");
    	if(a==c)
    		printf("B");
    	if(b==c)
    		printf("A");
    	return 0;
    }
```

#  03-3. 12-24小时制(15)

编写一个程序，要求用户输入24小时制的时间，然后显示12小时制的时间。

** 输入格式： **

输入在一行中给出带有中间的“:”符号（半角的冒号）的24小时制的时间，如 ` 12:34 ` 表示12点34分。当小时或分钟数小于10时，均没有前导的零，如
` 5:6 ` 表示5点零6分。

_ 提示：在scanf的格式字符串中加入“:”，让scanf来处理这个冒号。 _

** 输出格式： **

在一行中输出这个时间对应的12小时制的时间，数字部分格式与输入的相同，然后跟上空格，再跟上表示上午的字符串“AM”或表示下午的字符串“PM”。如“ `
5:6 PM ` ”表示下午5点零6分。注意，在英文的习惯中，中午12点被认为是下午，所以24小时制的 ` 12:00 ` 就是12小时制的 ` 12:0
PM ` ；而0点被认为是第二天的时间，所以是 ` 0:0 AM ` 。

** 输入样例： **
    
    
    21:11
    

** 输出样例： **
    
    
    9:11 PM
    

* * *
    
```C
    #include<stdio.h>
    
    int main()
    {
    	int hour,minute;
    	scanf("%d:%d",&hour,&minute);
    	if(hour<12)
    		printf("%d:%d AM",hour,minute);
    	else if(hour==12)
    		printf("%d:%d PM",hour,minute);
    	else
    		printf("%d:%d PM",hour%12,minute);
    	
    }
```

#  03-4. 成绩转换(15)

时间限制

400 ms  

内存限制

65536 kB  

代码长度限制

8000 B  

判题程序

Standard

作者

沈睿（浙江大学）  

本题要求编写程序将一个百分制成绩转换为五分制成绩。转换规则：

* 大于等于90分为A； 
* 小于90且大于等于80为B； 
* 小于80且大于等于70为C； 
* 小于70且大于等于60为D； 
* 小于60为E。 

** 输入格式： **

输入在一行中给出1个整数的百分制成绩。

** 输出格式： **

在一行中输出对应的五分制成绩。

** 输入样例： **
    
    
    90
    

** 输出样例： **
    
    
    A
    

* * *
```C
    
    #include<stdio.h>
    
    int main()
    {
    	int grade;
    	scanf("%d",&grade);
    	if(grade>=90)
    		printf("A");
    	else if(grade>=80)
    		printf("B");
    	else if(grade>=70)
    		printf("C");
    	else if(grade>=60)
    		printf("D");
    	else
    		printf("E");
    	return 0; 
    }
```
  
  
  

  

