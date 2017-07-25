---
title: java作业Date类
date: 2014-12-10 17:09:12
tags: 
  - java
  - Date
category: java
---


Date类

    
```JAVA    
    package wfx;
    
    public class Date {
    	private int year;
    	private int month;
    	private int day;
    	
    	private static int[] daysOfMonth = new int[]{
    		31,28,31,30,31,30,31,31,30,31,30,31
    	}; 							//每个月的天数
    	
    	public Date(){            	//无参构造方法
    		if(isLeapYear(this.year)){
    			daysOfMonth[1] = 29;
    		}
    	}
    	public Date(int year,int month,int day){ 				//有参构造方法
    		this.year = year;
    		this.month = month;
    		this.day = day;
    		if(isLeapYear(this.year)){
    			daysOfMonth[1] = 29;
    		}
    	}
    	
  <!-- more -->  	
    	
    	//get方法
    	public int getYear(){
    		return this.year;
    	}
    	public int getMonth(){
    		return this.month;
    	}
    	public int getDay(){
    		return this.day;
    	}
    	
    	//set方法
    	public void setYear(int year){
    		this.year = year;
    	}
    	public void setMonth(int month){
    		this.month = month;
    	}
    	public void setDay(int day){
    		this.day = day;
    	}
    	
    	//判断闰年
    	public boolean isLeapYear(int year){
    		return (year%4==0 && year%100!=0 || year%400==0);
    	}
    	
    	//判断输入是否合法
    	public boolean CheckDate(){
    		if(month < 1 || month > 12 || day < 1 || day > daysOfMonth[month-1]){
    			return false;
    		}
    		return true;
    	}
    	
    	//求这一天是周几
    	public void WeekDay(){
    		if(!CheckDate()){
    			System.out.println("Invalid date!!!");
    			return ;
    		}                         //判断合法
    		int sum = 0;
    		for(int i = 1; i < year; i++){  
                if(isLeapYear(i)){  
                    sum += 366;  
                }else{  
                    sum += 365;  
                }         
            }   
    		for(int i = 0; i < month - 1; i++){  
                    sum += daysOfMonth[i];             
            }
    		sum += day;
    		switch(sum % 7){  
            case 0: printDate(year,month,day);System.out.println(" is:" + "Sunday");break;  
            case 1: printDate(year,month,day);System.out.println(" is:" + "Monday");break;  
            case 2: printDate(year,month,day);System.out.println(" is:" + "Tuesday");break;  
            case 3: printDate(year,month,day);System.out.println(" is:" + "Wednsday");break;  
            case 4: printDate(year,month,day);System.out.println(" is:" + "Thursday");break;  
            case 5: printDate(year,month,day);System.out.println(" is:" + "Friday");break;  
            case 6: printDate(year,month,day);System.out.println(" is:" + "Saturday");break;       
            }
    		System.out.println();
    	}
    	
    	//求下一天
    	public void nextDate(){
    		if(!CheckDate()){  
                System.out.println("Invalid date!");  
                return;  
            }         
             
    		System.out.print("The nextDate is : " ); 
            if(day < daysOfMonth[month - 1]){
            	
            	printDate(year,month,day + 1);  
            }else{  
                if(month == 12){  
                	printDate(year + 1,1,1);  
                }else{  
                	printDate(year,month + 1,1);  
                }                 
            }   
            System.out.println();
        }
    		
    	//求上一天
    	public void preDate(){
    		if(!CheckDate()){  
                System.out.println("Invalid date!");  
                return;  
            }            
            if(day == 1){  
                if(month == 1){  
                	printDate(year - 1,12,31);  
                }else{  
                	printDate(year,month - 1,daysOfMonth[month - 2]);  
                }  
            }else{  
            	System.out.print("The proDate is : " );
            	printDate(year,month,day - 1);
            }
            System.out.println();
    	}
    	
    	//以一定格式打印日期
    	public void printDate(int year,int month,int day){
    		System.out.print(year + "-" + month + "-" + day + " ");
    	}
    }
    

  

  
测试类

    
    
    package wfx;
    
    import java.util.Scanner;
    
    public class Test {
    	public static void main(String[] args) {
    		Scanner input = new Scanner(System.in);
    		int year, month, day;
    		System.out.println("please enter the year,month and year: ");
    		year = input.nextInt();
    		month = input.nextInt();
    		day = input.nextInt();
    		Date date = new Date(year, month, day);
    		date.nextDate();
    		date.preDate();
    		date.WeekDay();
    	}
    }
    

  
```

