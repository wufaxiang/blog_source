---
title: 中国大学MOOC-翁恺-C语言程序习题第十周
date: 2015-01-22 14:48:12
tags: C
category: PAT-C
---

#  10-0. 说反话 (20)

给定一句英语，要求你编写程序，将句中所有单词的顺序颠倒输出。

** 输入格式： ** 测试输入包含一个测试用例，在一行内给出总长度不超过80的字符串。字符串由若干单词和若干空格组成，其中单词是由英文字母（大小写有区分）组成的字符串，单词之间用1个空格分开，输入保证句子末尾没有多余的空格。 

** 输出格式： ** 每个测试用例的输出占一行，输出倒序后的句子。 
<!-- more -->
** 输入样例： **
    
    
    Hello World Here I Come
    

** 输出样例： **
    
    
    Come I Here World Hello
    

* * *

分析：一开始做这道题时，第一想法就是用一个字符串数组，然后就这样做下去了，最后报错，囧
![委屈](http://static.blog.csdn.net/xheditor/xheditor_emot/default/wronged.gif)
，才发现原来c语言是没有string类型的，所以将程序改成了使用二维数组的方法，c语言中字符串是以字符数组的形式存在的，所以二维字符数组就可以解决啦

```C 
    
    #include<stdio.h>
    
    int main()
    {
        char str[40][80] = {0};            //定义二维数组 
        int i, j;                        //定义自变量 
        int out = 0;
        int cnt = 0;
        //为二维数组填充字符串 
        for (i = 0; i < 40; i++)
        {
            for (j = 0; j < 80; j++)
            {
                scanf("%c", &str[i][j]);
                if (' ' == str[i][j])
                {
                    str[i][j] = '\0';
                    cnt++;
                    break;
                }
                if ('\n' == str[i][j])
                {
                    str[i][j] = '\0';
                    out = 1; 
                    break;
                }    
            }
            if (out)
                break;
        }    
         
        //输出字符 
        while (cnt >= 0)
        {
            if (cnt > 0)
                printf("%s ", str[cnt]);
            else
                printf("%s", str[cnt]);    
            cnt--;    
        }
        printf("\n");
         
        return 0;
    }

```

#  10-1. 在字符串中查找指定字符(15)

输入一个字符串S，再输入一个字符c，要求在字符串S中查找字符c。如果找不到则输出“Not found”；若找到则输出字符串S中从c开始的所有字符。

** 输入格式： **

输入在第1行中给出一个不超过80个字符长度的、以回车结束的非空字符串；在第2行中给出一个字符。

** 输出格式： **

在一行中按照题目要求输出结果。

** 输入样例1： **
    
    
    It is a black box
    b
    

** 输出样例1： **
    
    
    black box
    

** 输入样例2： **
    
    
    It is a black box
    B
    

** 输出样例2： **
    
    
    Not found
    

* * *

分析：这题我没有去使用二维数组的方法，而是采用了get函数，然后对每个字符进行分析，这样比那种方法简单很多

    
```C
        #include <stdio.h>
        int main(int argc, char const *argv[])
        {
            char str[80];
            char c;
            int i, j, k=0, n=0;
             
            gets(str);
            scanf("%c",&c);
             
            for(i=0,n=0;str[i]!='\0';i++)
            {
                if(str[i]==c)
                {
                    n+=1;
                    printf("%s\n",&str[i]);
                    break;
                }
            }
         
            if(n==0)
            {
                printf("Not found\n");
            }
             
            return 0;
        }
```
  

#  10-2. 删除字符串中的子串(20)

输入2个字符串S1和S2，要求删除字符串S1中出现的所有子串S2，即结果字符串中不能包含S2。

** 输入格式： **

输入在2行中分别给出不超过80个字符长度的、以回车结束的2个非空字符串，对应S1和S2。

** 输出格式： **

在一行中输出删除字符串S1中出现的所有子串S2后的结果字符串。

** 输入样例： **
    
    
    Tomcat is a male ccatat
    cat
    

** 输出样例： **
    
    
    Tom is a male 
    

* * *

分析：这道题就是一个字符串函数的使用，代码很简单有米有，有一点需要注意的是定义数组是要定义成81，因为字符串还有个‘\0’.

```C
    
    #include <stdio.h>
    #include <string.h>
    int main()
    {
        char a[81], b[81], *p;
        gets(a);
        gets(b);   
        p=a;
        int len=strlen(b);
        while( p=strstr(a, b) ){
            *p=0;
            strcat(a,p+len);
        }
        printf("%s\n",a);
        return 0;
    }
```
  

#  10-3. 字符串逆序(15)

输入一个字符串，对该字符串进行逆序，输出逆序后的字符串。

** 输入格式： **

输入在一行中给出一个不超过80个字符长度的、以回车结束的非空字符串。

** 输出格式： **

在一行中输出逆序后的字符串。

** 输入样例： **
    
    
    Hello World!
    

** 输出样例： **
    
    
    !dlroW olleH
    

* * *

分析：就是读入字符串，然后求长度，在反向输出就好了

```C
    
    #include<stdio.h>
    #include<string.h>
    
    int main()
    {
        char S1[81];
        gets(S1);                       /*检查输入的字符串*/
        char* p=S1;
        //printf("%s",p);           /*检查字符串赋给指针后的字符*/
        int l=strlen(S1)-1;            /*字符串的长度是从1开始数,而数组是从0开始数所以减1*/
        while(l>=0)
        {  
            printf("%c",S1[l]);         /*这里“”内必须是%c因为*/
            l--;                    /*是字符的输出不是字符数组的输出*/
        }                              /*如果是字符数组的输出时用%s*/
        return 0;
    }

```

#  10-4. 字符串循环左移(20)

输入一个字符串和一个非负整数N，要求将字符串循环左移N次。

** 输入格式： **

输入在第1行中给出一个不超过100个字符长度的、以回车结束的非空字符串；第2行给出非负整数N。

** 输出格式： **

在一行中输出循环左移N次后的字符串。

** 输入样例： **
    
    
    Hello World!
    2
    

** 输出样例： **
    
    
    llo World!He
    

分析：说下我的思路，先读入这一行字符串，然后构造一个字符数组，存储那些小于序号N的字符，然后先输出s[i],再输出array[i]，就可以完成了。

需要注意一下的是如果N大于字符串的长度，那么N要换成N-len。具体代码如下：

```C 
    
    #include<stdio.h> 
    
    int main()
    {
    	char s[101];
    	int N;
    	gets(s);
    	scanf("%d",&N);
    	char *p=s;	
    	int i;	
    	int len = strlen(s);
    	if(N>len)
    		N=N-len;
    	char array[N];
    	for(i=0;i<N;i++)
    		array[i] = s[i];
    	for(i=N;i<len;i++)
    		printf("%c",s[i]);
    	for(i=0;i<N;i++)
    		printf("%c",array[i]);
    
    	return 0;
    } 
```
  
总结：通过这一章，主要学会了字符串和字符数组有关的使用，还有字符串函数的用法。  

