---
title: Have Fun with Numbers
date: 2015-02-27 18:24:12
tags: 
  - PAT
category: PAT
---

&emsp;&emsp;Notice that the number 123456789 is a 9-digit number consisting exactly the numbers from 1 to 9, with no duplication. Double it we will obtain 246913578, which happens to be another 9-digit number consisting exactly the numbers from 1 to 9, only in a different permutation. Check to see the result if we double it again!

&emsp;&emsp;Now you are suppose to check if there are more numbers with this property. That is, double a given number with k digits, you are to tell if the resulting number consists of only a permutation of the digits in the original number.
<!-- more -->
** Input Specification: **

&emsp;&emsp;Each input file contains one test case. Each case contains one positive integer with no more than 20 digits.

** Output Specification: **

&emsp;&emsp;For each test case, first print in a line "Yes" if doubling the input number gives a number that consists of only a permutation of the digits in the original number, or "No" if not. Then in the next line, print the doubled number.

** Sample Input: **
    
    
    1234567899
    

** Sample Output: **
    
    
    Yes
    2469135798
    

* * *

分析：这道题的意思呢，就是给一个不超过20位的数，然后乘以2，如果之后得到的数的组成与之前一样，顺序不同，那么就输出“Yes”，否则就“No”咯。要声明一下
，这道题如果用长整形之类的话，那么大数的乘法是会有误差的，可以自己先试试，你会发现得到的数会让你大吃一惊。所以我们需要以另外一种方式来解决，我将每一位作为一
个字符输入，然后再转化为整形数，存到一个整形数组中，接着通过两个存储了乘2前后数组成的数组比较，来判断是否相同，由此来输出答案，当然这种方法需要你自己写代码
来进行乘法运算。只是乘2，应该都会得。

code:

    
```C
    #include <stdio.h>
    #include <stdlib.h>
    
    int main()
    {
        char c;
        int cnt=0;    //计数器
        //输入数字
        int num[20];
        int a[10]={0};
        int b[10]={0};  //用于比较的数组a,b，其中元素为含有这个数的个数
        while(1)
        {
            scanf("%c",&c);
            if((int)c-48>=0 && (int)c-48<=9)
            {
                num[cnt] = (int)c-48;       //将数存到数组中
                a[num[cnt]]++;   //个数+1
                cnt++;
            }
            else
                break;
        }
        //对数进行*2处理
        int i;
        int flag =0; //进位
        for(i=cnt-1;i>=0;i--)   //从最低位开始
        {
            num[i] = num[i]*2+flag;     //*2后的数
            if(num[i]>9 && i!=0)
            {
                flag =1;    //进位为1
                num[i] = num[i]%10; //只留个位
            }
            else
                flag =0;
            if(num[0]<=9)
                b[num[i]]++;      //个数+1
        }
        //判断a，b数组是否相同
        int sign = 0;
        for(i=0;i<10;i++)
        {
            if(a[i]!=b[i])
            {
                sign = 1;
                break;
            }
        }
        //输出
        if(sign == 0)
            printf("Yes\n");
        else
            printf("No\n");
        for(i=0;i<cnt;i++)
        {
            printf("%d",num[i]);
        }
        return 0;
    }
    
```
  
  

