---
title: java练习1-10
date: 2014-12-10 17:19:12
tags: 
  - java
  - 练习
category: java
---

【程序  1  】  
题目：古典问题：有一对兔子，从出生后第  3  个月起每个月都生一对兔子，小兔子长到第三个月后每个月又生一对兔子，假如兔子都不死，问每个月的兔子总数为多少？  

//  这是一个菲波拉契数列问题

    
```JAVA  
    public class lianxi01 {
    	public static void main(String[] args) {
    		System.out.println("第1个月的兔子对数:    1");
    		System.out.println("第2个月的兔子对数:    1");
    		int f1 = 1, f2 = 1, f, M = 24;
    		for (int i = 3; i <= M; i++) {
    			f = f2;
    			f2 = f1 + f2;
    			f1 = f;
    			System.out.println("第" + i + "个月的兔子对数: " + f2);
    		}
    	}
    }
```
<!-- more -->
  
  

【程序  2  】  
题目：判断  101-200  之间有多少个素数，并输出所有素数。  
程序分析：判断素数的方法：用一个数分别去除  2  到  sqrt(  这个数  )  ，如果能被整除，则表明此数不是素数，反之是素数。  

```JAVA    
    
    public class lianxi02 {
    	public static void main(String[] args) {
    		int count = 0;
    		for (int i = 101; i < 200; i += 2) {
    			boolean b = false;
    			for (int j = 2; j <= Math.sqrt(i); j++) {
    				if (i % j == 0) {
    					b = false;
    					break;
    				} else {
    					b = true;
    				}
    			}
    			if (b == true) {
    				count++;
    				System.out.println(i);
    			}
    		}
    		System.out.println("素数个数是: " + count);
    	}
    }
```
  
题目：打印出所有的  "  水仙花数  "  ，所谓  "  水仙花数  "  是指一个三位数，其各位数字立方和等于该数本身。例如：  153  是一个
"  水仙花数  "  ，因为  153=1  的三次方＋  5  的三次方＋  3  的三次方。  

    
```JAVA    
    public class lianxi03 {
    	public static void main(String[] args) {
    		int b1, b2, b3;
    		for (int m = 101; m < 1000; m++) {
    			b3 = m / 100;
    			b2 = m % 100 / 10;
    			b1 = m % 10;
    			if ((b3 * b3 * b3 + b2 * b2 * b2 + b1 * b1 * b1) == m) {
    				System.out.println(m + "是一个水仙花数");
    			}
    		}
    	}
    }
```
  
  
【程序  4  】  
题目：将一个正整数分解质因数。例如：输入  90,  打印出  90=2*3*3*5  。  
程序分析：对  n  进行分解质因数，应先找到一个最小的质数  k  ，然后按下述步骤完成：  
(1)  如果这个质数恰等于  n  ，则说明分解质因数的过程已经结束，打印出即可。  
(2)  如果  n <> k  ，但  n  能被  k  整除，则应打印出  k  的值，并用  n  除以  k  的商  ,  作为新的正整数你
n,  重复执行第一步。  
(3)  如果  n  不能被  k  整除，则用  k+1  作为  k  的值  ,  重复执行第一步。  

```JAVA    
    
    import java.util.*;
    
    public class lianxi04 {
    	public static void main(String[] args) {
    		Scanner s = new Scanner(System.in);
    		System.out.print("请键入一个正整数:     ");
    		int n = s.nextInt();
    		int k = 2;
    		System.out.print(n + "=");
    		while (k <= n) {
    			if (k == n) {
    				System.out.println(n);
    				break;
    			} else if (n % k == 0) {
    				System.out.print(k + "*");
    				n = n / k;
    			} else
    				k++;
    		}
    	}
    }
```
  
题目：利用条件运算符的嵌套来完成此题：学习成绩  > =90  分的同学用  A  表示，  60-89  分之间的用  B  表示，  60  分以下的用 C  表示。  

    
```JAVA    
    import java.util.*;
    
    public class lianxi05 {
    	public static void main(String[] args) {
    		int x;
    		char grade;
    		Scanner s = new Scanner(System.in);
    		System.out.print("请输入一个成绩: ");
    		x = s.nextInt();
    		grade = x >= 90 ? 'A' : x >= 60 ? 'B' : 'C';
    		System.out.println("等级为：" + grade);
    
    	}
    }

```
题目：输入两个正整数  m  和  n  ，求其最大公约数和最小公倍数。  
/**  在循环中，只要除数不等于  0，用较大数除以较小的数，将小的一个数作为下一轮循环的大数，取得的余数作为下一轮循环的较小的数，如此循环直到较小的数的值为  0
，返回较大的数，此数即为最大公约数，最小公倍数为两数之积除以最大公约数。  * /  

```JAVA  
    import java.util.*;
    
    public class lianxi06 {
    	public static void main(String[] args) {
    		int a, b, m;
    		Scanner input = new Scanner(System.in);
    		System.out.print("键入一个整数： ");
    		a = input.nextInt();
    		System.out.print("再键入一个整数： ");
    		b = input.nextInt();
    		m = deff(a, b);
    		int n = a * b / m;
    		System.out.println("最大公约数: " + m);
    		System.out.println("最小公倍数: " + n);
    	}
    	
    	public static int deff(int x, int y) {
    		int t;
    		if (x < y) {
    			t = x;
    			x = y;
    			y = t;
    		}
    		while (y != 0) {
    			if (x == y)
    				return x;
    			else {
    				int k = x % y;
    				x = y;
    				y = k;
    			}
    		}
    		return x;
    	}
    }
```
  
【程序  7  】  
题目：输入一行字符，分别统计出其中英文字母、空格、数字和其它字符的个数。  

```JAVA    
    
    import java.util.*;
    
    public class lianxi07 {
    	public static void main(String[] args) {
    		int digital = 0;
    		int character = 0;
    		int other = 0;
    		int blank = 0;
    		char[] ch = null;
    		Scanner sc = new Scanner(System.in);
    		String s = sc.nextLine();
    		ch = s.toCharArray();
    		for (int i = 0; i < ch.length; i++) {
    			if (ch >= '0' && ch <= '9') {
    				digital++;
    			} else if ((ch >= 'a' && ch <= 'z') || ch > 'A' && ch <= 'Z') {
    				character++;
    			} else if (ch == ' ') {
    				blank++;
    			} else {
    				other++;
    			}
    		}
    		System.out.println("数字个数: " + digital);
    		System.out.println("英文字母个数: " + character);
    		System.out.println("空格个数: " + blank);
    		System.out.println("其他字符个数:" + other);
    
    	}
    
    }
```
【程序  8  】  
题目：求  s=a+aa+aaa+aaaa+aa...a  的值，其中  a  是一个数字。例如  2+22+222+2222+22222(  此时共有
5  个数相加  )  ，几个数相加有键盘控制。

```JAVA   
    
    import java.util.*;
    
    public class lianxi08 {
    	public static void main(String[] args) {
    		long a, b = 0, sum = 0;
    		Scanner s = new Scanner(System.in);
    		System.out.print("输入数字a的值： ");
    		a = s.nextInt();
    		System.out.print("输入相加的项数：");
    		int n = s.nextInt();
    		int i = 0;
    		while (i < n) {
    			b = b + a;
    			sum = sum + b;
    			a = a * 10;
    			++i;
    		}
    		System.out.println(sum);
    	}
    }

```
【程序  9  】  
题目：一个数如果恰好等于它的因子之和，这个数就称为  "  完数  "  。例如  6=1  ＋  2  ＋  3\.  编程  找出  1000
以内的所有完数。

```JAVA   
    
    public class lianxi09 {
    	public static void main(String[] args) {
    		System.out.println("1到1000的完数有： ");
    		for (int i = 1; i < 1000; i++) {
    			int t = 0;
    			for (int j = 1; j <= i / 2; j++) {
    				if (i % j == 0) {
    					t = t + j;
    				}
    			}
    			if (t == i) {
    				System.out.print(i + "     ");
    			}
    		}
    	}
    }

```

【程序  10】  
题目：一球从100米高度自由落下，每次落地后反跳回原高度的一半；再落下，求它在     第10次落地时，共经过多少米？第10次反弹多高？

    
```JAVA    
    public class lianxi10 {
    	public static void main(String[] args) {
    		double h = 100, s = 100;
    		for (int i = 1; i < 10; i++) {
    			s = s + h;
    			h = h / 2;
    		}
    		System.out.println("经过路程：" + s);
    		System.out.println("反弹高度：" + h / 2);
    	}
    }
```
  
  
  

