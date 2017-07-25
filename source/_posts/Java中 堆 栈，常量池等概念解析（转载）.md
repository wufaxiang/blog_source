---
title: Java中 堆 栈，常量池等概念解析（转载）
date: 2014-12-17 16:09:15
tags: 
  - java
  - 堆
  - 栈
category: java
---

1.寄存器：最快的存储区, 由编译器根据需求进行分配,我们在程序中无法控制.  
2\. 栈：存放基本类型的变量数据和对象的引用，但对象本身不存放在栈中，而是存放在堆（new 出来的对象）或者常量池中（字符串常量对象存放在常量池中。）  
3\. 堆：存放所有new出来的对象。  
4\. 静态域：存放静态成员（static定义的）  
5\. 常量池：存放字符串常量和基本类型常量（public static final）。  
6\. 非RAM存储：硬盘等永久存储空间  
这里我们主要关心栈，堆和常量池，对于栈和常量池中的对象可以共享，对于堆中的对象不可以共享。栈中的数据大小和生命周期是可以确定的，当没有引用指向数据时，这个数
据就会消失。堆中的对象的由垃圾回收器负责回收，因此大小和生命周期不需要确定，具有很大的灵活性。  
对于字符串：其对象的引用都是存储在栈中的，如果是编译期已经创建好(直接用双引号定义的)的就存储在常量池中，如果是运行期（new出来的）才能确定的就存储在堆中
。对于equals相等的字符串，在常量池中永远只有一份，在堆中有多份。  
如以下代码：
<!-- more -->
Java代码

```JAVA    
    String s1 = "china";  
    String s2 = "china";  
    String s3 = "china";  
    String ss1 = new String("china");  
    String ss2 = new String("china");  
    String ss3 = new String("china"); 
```
[ ![6020744_1](http://hi.csdn.net/attachment/201011/27/0_12908518495f7w.gif)
](http://hi.csdn.net/attachment/201011/27/0_1290851849Jk3x.gif)

这里解释一下黄色这3个箭头，对于通过new产生一个字符串（假设为”china”）时，会先去常量池中查找是否已经有了”china”对象，如果没有则在常量池中创
建一个此字符串对象，然后堆中再创建一个常量池中此”china”对象的拷贝对象。这也就是有道面试题：String s = new
String(“xyz”);产生几个对象？一个或两个，如果常量池中原来没有”xyz”,就是两个。

对于基础类型的变量和常量：变量和引用存储在栈中，常量存储在常量池中。  
如以下代码：

Java代码

```JAVA   
    int i1 = 9;  
    int i2 = 9;  
    int i3 = 9;   
    public static final int INT1 = 9;  
    public static final int INT2 = 9;  
    public static final int INT3 = 9; 
```
[ ![6020744_2](http://hi.csdn.net/attachment/201011/27/0_1290851850lLaN.gif)
](http://hi.csdn.net/attachment/201011/27/0_1290851849l57A.gif)  
对于成员变量和局部变量：成员变量就是方法外部，类的内部定义的变量；局部变量就是方法或语句块内部定义的变量。局部变量必须初始化。  
形式参数是局部变量，局部变量的数据存在于栈内存中。栈内存中的局部变量随着方法的消失而消失。  
成员变量存储在堆中的对象里面，由垃圾回收器负责回收。  
如以下代码：

Java代码

    
```JAVA  
    class BirthDate {  
        private int day;  
        private int month;  
        private int year;      
        public BirthDate(int d, int m, int y) {  
            day = d;   
            month = m;   
            year = y;  
        }  
        //省略get,set方法………  
    }  
    public class Test{  
        public static void main(String args[]){  
    int date = 9;  
            Test test = new Test();        
               test.change(date);   
            BirthDate d1= new BirthDate(7,7,1970);         
        }    
        public void change1(int i){  
            i = 1234;  
        } 
    } 

```

  
[ ![6020744_3](http://hi.csdn.net/attachment/201011/27/0_1290851850Sejt.gif)
](http://hi.csdn.net/attachment/201011/27/0_1290851850D5GG.gif)  
对于以上这段代码，date为局部变量，i,d,m,y都是形参为局部变量，day，month，year为成员变量。下面分析一下代码执行时候的变化：  
1\. main方法开始执行：int date = 9;  
date局部变量，基础类型，引用和值都存在栈中。  
2\. Test test = new Test();  
test为对象引用，存在栈中，对象(new Test())存在堆中。  
3\. test.change(date);  
i为局部变量，引用和值存在栈中。当方法change执行完成后，i就会从栈中消失。  
4\. BirthDate d1= new BirthDate(7,7,1970);  
d1 为对象引用，存在栈中，对象(new
BirthDate())存在堆中，其中d，m，y为局部变量存储在栈中，且它们的类型为基础类型，因此它们的数据也存储在栈中。
day,month,year为成员变量，它们存储在堆中(new
BirthDate()里面)。当BirthDate构造方法执行完之后，d,m,y将从栈中消失。  
5.main方法执行完之后，date变量，test，d1引用将从栈中消失，new Test(),new BirthDate()将等待垃圾回收。

  

  

** Java 中的堆和栈 **   
Java把内存划分成两种：一种是栈内存，一种是堆内存。

在函数中定义的一些基本类型的变量和对象的引用变量都在函数的栈内存 中分配  。

当在一段代码块定义一个变量时，Java就在栈中为这个变量分配内存空间，当超过变量的作用域后，Java会自动释放掉为该变量所分配的内存空间，
该内存空间可以立即被另作他用。  
  
堆内存用来存放由 new创建的对象和数组。  
  
在堆中分配的内存，由Java虚拟机的自动垃圾回收器来管理。  
  
在堆中产生了一个数组或对象后，还可以在栈中定义一个特殊的变量，让栈中这个变量的取值等于数组或对象在堆内存中的首地址，栈中的这个变量就成了数组或对
象的引用变量。  
  
引用变量就相当于是为数组或对象起的一个名称，以后就可以在程序中使用栈中的引用变量来访问堆中的数组或对象。  
  
具体的说：  
栈与堆都是Java用来在Ram中存放数据的地方。与C++不同，Java自动管理栈和堆，程序员不能直接地设置栈或堆。  
Java的堆是一个运行时数据区,类的(对象从中分配空间。这些对象通过new、newarray、anewarray和multianewarray等
指令建立，它们不需要程序代码来显式的释放。堆是由垃圾回收来负责的，堆的优势是可以动态地分配内存大小，生存期也不必事先告诉编译器，因为它是在运行时
动态分配内存的，Java的垃圾收集器会自动收走这些不再使用的数据。但缺点是，由于要在运行时动态分配内存，存取速度较慢。  
栈的优势是，存取速度比堆要快，仅次于寄存器，栈数据可以共享。但缺点是，存在栈中的数据大小与生存期必须是确定的，缺乏灵活性。栈中主要存放一些基本类
型的变量（,int, short, long, byte, float, double, boolean, char）和对象句柄。  
栈有一个很重要的特殊性，就是存在栈中的数据可以共享  。 假设我们同时定义：  

	int a = 3;  
	int b = 3；  
编译器先处理int a = 3；首先它会在栈中创建一个变量为a的引用，然后查找栈中是否有3这个值，如果没找到，就将3存放进来，然后将a指向3。接着处理int
b = 3；在创建完b的引用变量后，因为在栈中已经有3这个值，便将b直接指向3。这样，就出现了a与b同时均指向3的情况。这时，如果再令a=4；那么编译器
会重新搜索栈中是否有4值，如果没有，则将4存放进来，并令a指向4；如果已经有了，则直接将a指向这个地址。因此a值的改变不会影响到b的值。要注意这
种数据的共享与两个对象的引用同时指向一个对象的这种共享是不同的，因为这种情况a的修改并不会影响到b,
它是由编译器完成的，它有利于节省空间。而一个对象引用变量修改了这个对象的内部状态，会影响到另一个对象引用变量。  
  
String是一 个特殊的包装类数据。可以用：  

	String str = new String("abc");  
	String str = "abc";  
两种的形式来创建，第一种是用new()来新 建对象的，它会在存放于堆中。每调用一次就会创建一个新的对象。  
而第二种是先在栈中创建一个对String类的对象引用变量str，然后查找栈
中有没有存放"abc"，如果没有，则将"abc"存放进栈，并令str指向”abc”，如果已经有”abc” 则直接令str指向“abc”。  
  
比较类里面的数值是否相等时，用equals()方法；当 测试两个包装类的引用是否指向同一个对象时，用==，  下面用例子说明上面的理论。  

	String str1 = "abc";  
	String str2 = "abc";  
	System.out.println(str1==str2); //true  
可以看出str1和 str2是指向同一个对象的。  
  
	String str1 =new String ("abc");  
	String str2 =new String ("abc");  
	System.out.println(str1==str2); // false  
用new的方式是生成不同的对象。每一次生成一个  。  
因此用第二种方式创建多个”abc”字符串,在内存中其实只存在一个对象而已. 这种写法有利与节省内存空间.
同时它可以在一定程度上提高程序的运行速度，因为JVM会自动根据栈中数据的实际情况来决定是否有必要创建新对象。而对于String str = new
String("abc")；的代码，则一概在堆中创建新对象，而不管其字符串值是否相等，是否有必要创建新对象，从而加重了程序的负担。  
另一方面, 要注意: 我们在使用诸如String str =
"abc"；的格式定义类时，总是想当然地认为，创建了String类的对象str。担心陷阱！对象可能并没有被创建！而可能只是指向一个先前已经创建的
对象。只有通过new()方法才能保证每次都创建一个新的对象。由于String类的immutable性质，当String变量需要经常变换其值时，应
该考虑使用StringBuffer类，以提高程序效率。

