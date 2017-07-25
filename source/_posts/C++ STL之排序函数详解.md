---
title: C++ STL之排序函数详解
date: 2015-01-22 22:25:12
tags: 
  - C++
  - STL
  - 排序
category: C++
---

##  ** 排序函数： **

函数名

功能描述

> sort

对给定区间所有元素进行排序

> stable_sort

对给定区间所有元素进行稳定排序

> partial_sort

对给定区间所有元素部分排序

> partial_sort_copy

<!-- more -->
对给定区间复制并排序

> nth_element

找出给定区间的某个位置对应的元素

> is_sorted

判断一个区间是否已经排好序

> partition

使得符合某个条件的元素放在前面

> stable_partition

相对稳定的使得符合某个条件的元素放在前面
 
  

##  ** sort: **

要使用此函数只需用#include <algorithm> sort即可使用，  
语法描述为：

sort(begin,end)，表示一个范围，例如：  

    
```C++    
    int main()
    {
    	int a[]={2,4,1,23,5,76,0,43,24,65}; 
    	int i; 
    	for(i=0;i<10;i++)
    		cout<<a[i]<<" ";
    	cout<<endl;
    	sort(a,a+10);
    	for(i=0;i<10;i++)
    		cout<<a[i]<<" ";
    	return 0;
    }
```
输出结果：  

	2 4 1 23 5 76 0 43 24 65  
	0 1 2 4 5 23 24 43 65 76  
 

&emsp;&emsp;输出结果将是把数组a按升序排序，说到这里可能就有人会问怎么样用它降序排列呢？这就是下一个讨论的内容.

&emsp;&emsp;一种是自己编写一个比较函数来实现，接着调用三个参数的sort：sort(begin,end,compare)就成了。  
对于list容器，这个方法也适用，把compare作为sort的参数就可以了，即：sort(compare).  
1）自己编写compare函数：  

    
```C++    
    bool compare(int a,int b)
    {
          return a>b;   //降序排列，如果改为return a<b，则为升序
    }
```
输出结果  

	2 4 1 23 5 76 0 43 24 65  
	76 65 43 24 23 5 4 2 1 0  

  

2）更进一步，让这种操作更加能适应变化。也就是说，能给比较函数一个参数，用来指示是按升序还是按降序排,这回轮到函数对象出场了。

为了描述方便，我先定义一个枚举类型EnumComp用来表示升序和降序。很简单：

** enum Enumcomp{ASC,DESC}; **

然后开始用一个类来描述这个函数对象。它会根据它的参数来决定是采用“<”还是“>”。  

    
```C++    
    class compare
    {
          private:
                Enumcomp comp;
          public:
                compare(Enumcomp c):comp(c) {};
          bool operator () (int num1,int num2)
             {
                switch(comp)
                  {
                     case ASC:
                            return num1<num2;
                     case DESC:
                            return num1>num2;
                  }
              }
    };

```
接下来使用 ** sort(begin,end,compare(ASC) ** 实现升序， ** sort(begin,end,compare(DESC)
** 实现降序  
  

3)其实对于这么简单的任务（类型支持“<”、“>”等比较运算符），完全没必要自己写一个类出来。  
标准库里已经有现成的了，就在functional里，include进来就行了。functional提供了一堆基于模板的比较函数对象。  
它们是（看名字就知道意思了）： ** equal_to<Type> ** 、 ** not_equal_to<Type>、greater<Type>、gre
ater_equal<Type>、less<Type>、less_equal<Type> ** 。  
对于这个问题来说，greater和less就足够了，直接拿过来用：

升序： ** sort(begin,end,less<data-type>()); **  
降序： ** sort(begin,end,greater<data-type>()). **  

```C++    
    
    int main()
    {
    	int a[]={2,4,1,23,5,76,0,43,24,65}; 
    	int i; 
    	for(i=0;i<10;i++)
    		cout<<a[i]<<" ";
    	cout<<endl;
    	sort(a,a+10,greater<int>());  //升序为less<int>() 
    	for(i=0;i<10;i++)
    		cout<<a[i]<<" ";
    	return 0;
    }

```
  
4)既然有迭代器，如果是string 就可以使用反向迭代器来完成逆序排列，程序如下：  

    
```C++    
    int main()
    {
         string str("cvicses");
         string s(str.rbegin(),str.rend());
         cout << s <<endl;
         return 0;
    }

```
 

##  stable_sort:

stable_sort 函数与sort函数用法相同。这两个函数的原理都是快速排序，时间复杂度在所有排序中最低，为O(nlog2n) ;

区别是stable_sort 是稳定排序，也就是stable_sort函数遇到两个数相等时，不对其交换顺序；

这个应用在数组里面不受影响，当函数参数传入的是结构体时，会发现两者之间的明显区别；

  

##  ** partial_sort: **  

paitical_sort的原理是堆排序！

首先创建一个堆，得到最大值。如果要得到次大值，就将头结点去掉，即调用pop_heap()，此时的头结点就是次大值，可以这样依次得到最大或者最小的几个值！

函数原型有：

> partial_sort(beg,mid,end)

> partial_sort(beg,mid,end,comp)  

** 函数作用： **

对mid-beg个元素进行排序，也就是说，如果mid-beg等于42，则该函数将有序次序中的最小值元素放在序列中

的前42个位置。partial_sort完成之后，从beg到mid(但不包括mid)范围内的元素时有序的，已排序范围内没有

元素大于mid之后的元素。未排序元素之间的次序是未指定的。

例如：  
有一个赛跑成绩的集合，我们想知道前三名的成绩但并不关心其他名次的次序，可以这样对这个序列进行排序。  
** partial_sort（scores.begin(),scores.begin()+3,scores.end()); **
    
```C++    
    #include <vector>  
    #include <iterator>  
    #include <iostream>  
    #include <algorithm>  
    #include <functional>  
    #include <cstdlib>  
    #include <time.h>  
    using namespace std;  
      
    int rand_int()  
    {  
        return rand()%100;  
    }  
      
    void print(vector<int> &v,const char* s)  
    {  
        cout<<s<<endl;  
        copy(v.begin(),v.end(),ostream_iterator<int>(cout," "));  
        cout<<endl;  
    }  
      
    bool cmp(int &a, int &b)  
    {  
        if(a>b)  
            return true;  
        return false;  
    }  
      
    class compare{  
    public:  
        bool operator()(const int &a,const int &b)  
        {  
            if(a<b)  
                return true;  
            return false;  
        }  
    };  
      
    int main()  
    {  
        srand(time(NULL));  
        vector<int> v;  
        generate_n(back_inserter(v),10,rand_int);  
        print(v,"产生10个随机数");  
      
        partial_sort(v.begin(),v.begin()+4,v.end());  
        print(v,"局部递增排序");  
      
        partial_sort(v.begin(),v.begin()+4,v.end(),cmp);  
        print(v,"局部递减排序");  
      
        partial_sort(v.begin(),v.begin()+4,v.end(),compare());  
        print(v,"局部递增排序");  
      
        return 0;  
    }

```
测试结果：

	产生10个随机数  
	69 54 33 50 13 52 54 61 29 5  
	局部递增排序  
	5 13 29 33 69 54 54 61 52 50  
	局部递减排序  
	69 61 54 54 5 13 29 33 52 50  
	局部递增排序  
	5 13 29 33 69 61 54 54 52 50  
  


通过程序可以看到两次局部递增排序并不相同，因为partial_port不是稳定排序算法。在只需要最大或最小的几个值时，partial_port比其他排序算法
快。

  

  

##  ** partial_sort_copy: **

原型：

```C++    
    
     template<class InputIterator, RandomAccessIterator> inline
       RandomAccessIterator partial_sort(InputIterator first1,
                                         InputIterator last1,
                                         RandomAccessIterator first2,
                                         RandomAccessIterator last2)
```
说明：

partial_sort_copy其实是copy和partial_sort的组合。被排序(被复制)的数量是[first,last)和[result_first, result_last)中区间较小的那个。如果[result_first,result_last)区间大于[first, last)区间，那么partial_sort相当于copy和sort的组合。  

示例代码：

    
```C++    
    #include <iostream>
    #include <algorithm>
    #include <functional>
    #include <vector>
    using namespace std;
    
    void main()
    {
        const int VECTOR_SIZE = 8 ;
    
        // Define a template class vector of int
        typedef vector<int, allocator<int> > IntVector ;
    
        //Define an iterator for template class vector of strings
        typedef IntVector::iterator IntVectorIt ;
    
        IntVector Numbers(VECTOR_SIZE) ;
        IntVector Result(4) ;
    
        IntVectorIt start, end, it ;
    
        // Initialize vector Numbers
        Numbers[0] = 4 ;
        Numbers[1] = 10;
        Numbers[2] = 70 ;
        Numbers[3] = 30 ;
        Numbers[4] = 10;
        Numbers[5] = 69 ;
        Numbers[6] = 96 ;
        Numbers[7] = 7;
    
        start = Numbers.begin() ;   // location of first
                                    // element of Numbers
    
        end = Numbers.end() ;       // one past the location
                                    // last element of Numbers
    
        cout << "Before calling partial_sort_copy\n" << endl ;
    
        // print content of Numbers
        cout << "Numbers { " ;
        for(it = start; it != end; it++)
            cout << *it << " " ;
        cout << " }\n" << endl ;
    
        // sort the smallest 4 elements in the Numbers
        // and copy the results in Result
        partial_sort_copy(start, end, Result.begin(), Result.end()) ;
    
        cout << "After calling partial_sort_copy\n" << endl ;
    
        cout << "Numbers { " ;
        for(it = start; it != end; it++)
            cout << *it << " " ;
        cout << " }\n" << endl ;
    
        cout << "Result { " ;
        for(it = Result.begin(); it != Result.end(); it++)
            cout << *it << " " ;
        cout << " }\n" << endl ;
    }
```
测试结果：

	Before calling partial_sort_copy  
	  
	Numbers { 4 10 70 30 10 69 96 7  }  
	  
	After calling partial_sort_copy  
	  
	Numbers { 4 10 70 30 10 69 96 7  }  
	  
	Result { 4 7 10 10  }  
  
  


  

##  nth_element:

简单的说nth_element算法仅排序第nth个元素（从0开始的索引）

如iarray [first,last) 元素区间

排序后  iarray[nth] 就是第nth大的元素（从0开始）

要注意的是[first,nth) [nth,last)内 的大小循序还不一定

只能确定iarray[nth]是第nth大的元素。

当然 [first,nth) 肯定是不大于 [nth,last)的。

简单测试代码如下

要注意的是，此函数只是将第nth大的元素排好了位置，但并没有返回值

所以要知道第nth大的元素 还得进行一步，cout<<iarray[nth]<<endl; nth既那个位子

    
```C++
    #include <iostream>
    #include <algorithm>
    using namespace std;
    
    int main()
    {
        int iarray[]={5,6,15,89,7,2,1,3,52,63,12,64,47};
        int len=sizeof(iarray)/sizeof(int);
        int i;
        for(i=0;i<len;i++)
           cout<<iarray[i]<<" ";
        nth_element(iarray,iarray+6,iarray+len);         //排序第6个元素
            cout<<endl;
        for(i=0;i<len;i++)
           cout<<iarray[i]<<" ";
        cout<<endl;
        cout<<"第6-th个元素: "<<iarray[6]<<endl;
    }
    
```
测试结果：

	5 6 15 89 7 2 1 3 52 63 12 64 47  
	5 3 1 2 6 7 12 15 47 63 52 64 89  
	第6-th个元素: 12  
  


  

##  is_sorted:

is_sorted用于判断一个区间是否已经有序，和排序一样，可以自己定义比较函数。  
但是，正如文章题目所说的一样，is_sorted是SGI
STL的扩展，在现在的GCC下并没有（VS下也没有），所以，为了保证代码的可移植性，最好不要使用这个函数。

##  

##  ** Partition ** :

作用：  
Partition 函数用于返回一个 Variant (String)，指定一个范围，在一系列计算的范围中指定的数字出现在这个范围内。  
函数的语法格式：  
Partition(number, start, stop, interval)  
Partition 函数的语法含有下面这些命名参数：部分描述  
number必要参数。整数，在所有范围中判断这个整数是否出现。  
start必要参数。整数，数值范围的开始值，这个数值不能小于 0。  
stop必要参数。整数，数值范围的结束值，这个数值不能等于或小于 start。  
说明：  
Partition 函数会标识 number 值出现的特定范围，并返回一个 Variant (String) 来描述这个范围。  
Partition 函数在查询中是最有用的。可以创建一个选择查询显示有多少定单落在几个变化的范围内，例如，定单数从 1 到 1000、1001 到
2000，以此类推。

  

解决问题：  

快速排序算法里的partition函数用来解决这样一个问题：给定一个数组arr[]和数组中任意一个元素a，重排数组使得a左边都小于它，右边都不小于它。

    
```C++    
    // arr[]为数组，start、end分别为数组第一个元素和最后一个元素的索引
     // povitIndex为数组中任意选中的数的索引
    int partition(int arr[], int start, int end, int pivotIndex)
    {
        int pivot = arr[pivotIndex];
        swap(arr[pivotIndex], arr[end]);
        int storeIndex = start;
        //这个循环比一般的写法简洁高效，呵呵维基百科上看到的
        for(int i = start; i < end; ++i) {
            if(arr[i] < pivot) {
                swap(arr[i], arr[storeIndex]);
                ++storeIndex;
            }
        }
        swap(arr[storeIndex], arr[end]);
        return storeIndex;
    }
```
  

##  

##  stable_partition：

stable_partition 就是partition排序算法的稳定算法，区别于 sort和stable_sort的区别是一样

  

  

##  选择合适的排序函数

为什么要选择合适的排序函数？可能你并不关心效率(这里的效率指的是程序运行时间), 或者说你的数据量很小， 因此你觉得随便用哪个函数都无关紧要。

其实不然，即使你不关心效率，如果你选择合适的排序函数，你会让你的代码更容易让人明白，你会让你的代码更有扩充性，逐渐养成一个良好的习惯，很重要吧 。

如果你以前有用过C语言中的qsort, 想知道qsort和他们的比较，那我告诉你，qsort和sort是一样的，因为他们采用的都是快速排序。从效率上看，以下
几种sort算法的是一个排序，效率由高到低（耗时由小变大）：

  1. partion 
  2. stable_partition 
  3. nth_element 
  4. partial_sort 
  5. sort 
  6. stable_sort 
 
Effective STL的文章, 其中对 [ 如何选择排序函数 ](http://stl.winterxy.com/html/000026.html) 总结的很好：

  * 若需对vector, string, deque, 或 array容器进行全排序，你可选择sort或stable_sort； 
  * 若只需对vector, string, deque, 或 array容器中取得top n的元素，部分排序partial_sort是首选. 
  * 若对于vector, string, deque, 或array容器，你需要找到第n个位置的元素或者你需要得到top n且不关系top n中的内部顺序，nth_element是最理想的； 
  * 若你需要从标准序列容器或者array中把满足某个条件或者不满足某个条件的元素分开，你最好使用partition或stable_partition； 
  * 若使用的list容器，你可以直接使用partition和stable_partition算法，你可以使用list::sort代替sort和stable_sort排序。若你需要得到partial_sort或nth_element的排序效果，你必须间接使用。正如上面介绍的有几种方式可以选择。 
总之记住一句话： ** 如果你想节约时间，不要走弯路, 也不要走多余的路! **

  

  

  

