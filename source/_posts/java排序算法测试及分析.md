---
title: java排序算法测试及分析
date: 2015-03-25 08:56:12
tags: 
  - java
  - 算法
category: java
---

老师布了一次要我们用数据分析排序算法的作业，学校里事情蛮多的，好久也没发博文了，也不能愉快的刷题了，这次就将作业放上，可能有些地方做的不好，请指正哈  

这次测试的是6种排序算法，冒泡，选择，插入，归并，堆以及快速排序，先上源码咯

##  1.冒泡排序：

    
```JAVA    
    package com.sort;
    
    import java.util.Random;
    
    public class BubbleSort{
    	public static <E extends Comparable<E>> void bubbleSort(E[] list) {
    		boolean needNextPass = true;
    		for(int k=1;k<list.length && needNextPass;k++){
    			needNextPass = false;
    			for(int i=0;i<list.length-1;i++){
    				if(list[i].compareTo(list[i+1])>0){
    					E temp = list[i];
    					list[i] = list[i+1];
    					list[i+1] = temp;
    					needNextPass = true;
    				}
    			}
    		}
    	}
 <!-- more -->   	
    	public static void main(String[] args){
    		long startTime,endTime,time;
    		int sortNumber;		//需要排序的数目
    		Random random = new Random();
    		for(sortNumber=10;sortNumber<=10000000;sortNumber*=10){
    			System.out.println(sortNumber+"个数据运行1次的时间:");
    			Integer[] list = new Integer[sortNumber];
    			//将数据存到数组中
    			for(int i=0;i<sortNumber;i++){
    				list[i] = random.nextInt(sortNumber);
    			}
    			startTime = System.nanoTime();
    			bubbleSort(list);
    			endTime = System.nanoTime();
    			time = endTime - startTime;
    			System.out.println("冒泡排序:"+time+"纳秒");
    		}
    	}
    }

```

##  2.选择排序：

```JAVA
    
    package com.sort;
    
    import java.util.Random;
    
    public class SelectionSort {
    	public static <E extends Comparable<E>> void selectionSort(E[] list) {
    		for (int i = list.length - 1; i >= 1; i--) {
    			// Find the maximum in the list[0..i]
    			E currentMax = list[0];
    			int currentMaxIndex = 0;
    
    			for (int j = 1; j <= i; j++) {
    				if (currentMax.compareTo(list[j]) < 0) {
    					currentMax = list[j];
    					currentMaxIndex = j;
    				}
    			}
    
    			// Swap list[i] with list[currentMaxIndex] if necessary;
    			if (currentMaxIndex != i) {
    				list[currentMaxIndex] = list[i];
    				list[i] = currentMax;
    			}
    		}
    	}
    	
    	public static void main(String[] args){
    		long startTime,endTime,time;
    		int sortNumber;		//需要排序的数目
    		Random random = new Random();
    		for(sortNumber=10;sortNumber<=10000000;sortNumber*=10){
    			System.out.println(sortNumber+"个数据运行1次的时间:");
    			Integer[] list = new Integer[sortNumber];
    			//将数据存到数组中
    			for(int i=0;i<sortNumber;i++){
    				list[i] = random.nextInt(sortNumber);
    			}
    			startTime = System.nanoTime();
    			selectionSort(list);
    			endTime = System.nanoTime();
    			time = endTime - startTime;
    			System.out.println("选择排序:"+time+"纳秒");
    		}
    	}
    }

```

##  3。插入排序：

```JAVA
    
    package com.sort;
    
    import java.util.Random;
    
    public class InsertSort {
    	public static <E extends Comparable<E>> void insertSort(E[] list) {
    		for (int i = 1; i < list.length; i++) {
    			E currentElement = list[i];
    			int k;
    			for (k = i - 1; k >= 0 && list[k].compareTo(currentElement) > 0; k--) {
    				list[k + 1] = list[k];
    			}
    
    			// Insert the current element into list[k+1]
    			list[k + 1] = currentElement;
    		}
    	}
    	
    	public static void main(String[] args){
    		long startTime,endTime,time;
    		int sortNumber;		//需要排序的数目
    		Random random = new Random();
    		for(sortNumber=10;sortNumber<=10000000;sortNumber*=10){
    			System.out.println(sortNumber+"个数据运行1次的时间:");
    			Integer[] list = new Integer[sortNumber];
    			//将数据存到数组中
    			for(int i=0;i<sortNumber;i++){
    				list[i] = random.nextInt(sortNumber);
    			}
    			startTime = System.nanoTime();
    			insertSort(list);
    			endTime = System.nanoTime();
    			time = endTime - startTime;
    			System.out.println("插入排序:"+time+"纳秒");
    		}
    	}
    }

```

##  4.归并排序：

```JAVA
    
    package com.sort;
    
    import java.util.Random;
    
    public class MergeSort {
    	@SuppressWarnings("unchecked")
    	public static <E extends Comparable<E>> void mergeSort(E[] list) {
    		if (list.length > 1) {
    			// Merge sort the first half
    			E[] firstHalf = (E[]) new Comparable[list.length / 2];
    			System.arraycopy(list, 0, firstHalf, 0, list.length / 2);
    			mergeSort(firstHalf);
    
    			// Merge sort the second half
    			int secondHalfLength = list.length - list.length / 2;
    			E[] secondHalf = (E[]) new Comparable[secondHalfLength];
    			System.arraycopy(list, list.length / 2, secondHalf, 0,
    					secondHalfLength);
    			mergeSort(secondHalf);
    
    			// Merge firstHalf with secondHalf
    			E[] temp = merge(firstHalf, secondHalf);
    			System.arraycopy(temp, 0, list, 0, temp.length);
    		}
    	}
    
    	@SuppressWarnings("unchecked")
    	private static <E extends Comparable<E>> E[] merge(E[] list1, E[] list2) {
    		E[] temp = (E[]) new Comparable[list1.length + list2.length];
    
    		int current1 = 0; // Index in list1
    		int current2 = 0; // Index in list2
    		int current3 = 0; // Index in temp
    
    		while (current1 < list1.length && current2 < list2.length) {
    			if (list1[current1].compareTo(list2[current2]) < 0) {
    				temp[current3++] = list1[current1++];
    			} else {
    				temp[current3++] = list2[current2++];
    			}
    		}
    
    		while (current1 < list1.length) {
    			temp[current3++] = list1[current1++];
    		}
    		while (current2 < list2.length) {
    			temp[current3++] = list2[current2++];
    		}
    
    		return temp;
    	}
    	
    	public static void main(String[] args){
    		long startTime,endTime,time;
    		int sortNumber;		//需要排序的数目
    		Random random = new Random();
    		for(sortNumber=10;sortNumber<=10000000;sortNumber*=10){
    			System.out.println(sortNumber+"个数据运行1次的时间:");
    			Integer[] list = new Integer[sortNumber];
    			//将数据存到数组中
    			for(int i=0;i<sortNumber;i++){
    				list[i] = random.nextInt(sortNumber);
    			}
    			startTime = System.nanoTime();
    			mergeSort(list);
    			endTime = System.nanoTime();
    			time = endTime - startTime;
    			System.out.println("归并排序:"+time+"纳秒");
    		}
    	}
    }

```

##  5.堆排序

```JAVA
    
    package com.sort;
    
    import java.util.Random;
    
    public class HeapSort {
    	public static <E extends Comparable<E>> void heapSort(E[] list) {
    		Heap<E> heap = new Heap<E>(); // Create a Heap
    
    		// Add elements to the heap
    		for (int i = 0; i < list.length; i++) {
    			heap.add(list[i]);
    		}
    
    		// Remove elements from the heap
    		for (int i = list.length - 1; i >= 0; i--) {
    			list[i] = heap.remove();
    		}
    	}
    
    	static class Heap<E extends Comparable<E>> {
    		private java.util.ArrayList<E> list = new java.util.ArrayList<E>();
    
    		/** Create a default heap */
    		public Heap() {
    		}
    
    		/** Create a heap from an array of objects */
    		public Heap(E[] objects) {
    			for (int i = 0; i < objects.length; i++) {
    				add(objects[i]);
    			}
    		}
    
    		/** Add a new object into the heap */
    		public void add(E newObject) {
    			list.add(newObject); // Append to the heap
    			int currentIndex = list.size() - 1; // The index of the last node
    
    			while (currentIndex > 0) {
    				int parentIndex = (currentIndex - 1) / 2;
    				// Swap if the current object is greater than its parent
    				if (list.get(currentIndex).compareTo(list.get(parentIndex)) > 0) {
    					E temp = list.get(currentIndex);
    					list.set(currentIndex, list.get(parentIndex));
    					list.set(parentIndex, temp);
    				} else {
    					break; // the tree is a heap now
    				}
    
    				currentIndex = parentIndex;
    			}
    		}
    
    		/** Remove the root from the heap */
    		public E remove() {
    			if (list.size() == 0) {
    				return null;
    			}
    
    			E removedObject = list.get(0);
    			list.set(0, list.get(list.size() - 1));
    			list.remove(list.size() - 1);
    
    			int currentIndex = 0;
    			while (currentIndex < list.size()) {
    				int leftChildIndex = 2 * currentIndex + 1;
    				int rightChildIndex = 2 * currentIndex + 2;
    
    				// Find the maximum between two children
    				if (leftChildIndex >= list.size()) {
    					break; // The tree is a heap
    				}
    				int maxIndex = leftChildIndex;
    				if (rightChildIndex < list.size()) {
    					if (((Comparable<E>) (list.get(maxIndex))).compareTo(list
    							.get(rightChildIndex)) < 0) {
    						maxIndex = rightChildIndex;
    					}
    				}
    
    				// Swap if the current node is less than the maximum
    				if (((Comparable<E>) (list.get(currentIndex))).compareTo(list
    						.get(maxIndex)) < 0) {
    					E temp = list.get(maxIndex);
    					list.set(maxIndex, list.get(currentIndex));
    					list.set(currentIndex, temp);
    					currentIndex = maxIndex;
    				} else {
    					break; // The tree is a heap
    				}
    			}
    
    			return removedObject;
    		}
    
    		/** Get the number of nodes in the tree */
    		public int getSize() {
    			return list.size();
    		}
    	}
    	
    	
    	public static void main(String[] args){
    		long startTime,endTime,time;
    		int sortNumber;		//需要排序的数目
    		Random random = new Random();
    		for(sortNumber=10;sortNumber<=10000000;sortNumber*=10){
    			System.out.println(sortNumber+"个数据运行1次的时间:");
    			Integer[] list = new Integer[sortNumber];
    			//将数据存到数组中
    			for(int i=0;i<sortNumber;i++){
    				list[i] = random.nextInt(sortNumber);
    			}
    			startTime = System.nanoTime();
    			heapSort(list);
    			endTime = System.nanoTime();
    			time = endTime - startTime;
    			System.out.println("堆排序:"+time+"纳秒");
    		}
    	}
    }

```

##  6.快速排序：

```JAVA
    
    package com.sort;
    
    import java.util.Random;
    
    public class QuickSort {
    	public static <E extends Comparable<E>> void quickSort(E[] list) {
    		quickSort(list, 0, list.length - 1);
    	}
    
    	private static <E extends Comparable<E>> void quickSort(E[] list,
    			int first, int last) {
    		if (last > first) {
    			int pivotIndex = partition(list, first, last);
    			quickSort(list, first, pivotIndex - 1);
    			quickSort(list, pivotIndex + 1, last);
    		}
    	}
    
    	/** Partition the array list[first..last] */
    	private static <E extends Comparable<E>> int partition(E[] list, int first,
    			int last) {
    		E pivot = list[first]; // Choose the first element as the pivot
    		int low = first + 1; // Index for forward search
    		int high = last; // Index for backward search
    
    		while (high > low) {
    			// Search forward from left
    			while (low <= high && list[low].compareTo(pivot) <= 0) {
    				low++;
    			}
    
    			// Search backward from right
    			while (low <= high && list[high].compareTo(pivot) > 0) {
    				high--;
    			}
    
    			// Swap two elements in the list
    			if (high > low) {
    				E temp = list[high];
    				list[high] = list[low];
    				list[low] = temp;
    			}
    		}
    
    		while (high > first && list[high].compareTo(pivot) >= 0) {
    			high--;
    		}
    
    		// Swap pivot with list[high]
    		if (pivot.compareTo(list[high]) > 0) {
    			list[first] = list[high];
    			list[high] = pivot;
    			return high;
    		} else {
    			return first;
    		}
    	}
    	
    	public static void main(String[] args){
    		long startTime,endTime,time;
    		int sortNumber;		//需要排序的数目
    		Random random = new Random();
    		for(sortNumber=10;sortNumber<=10000000;sortNumber*=10){
    			System.out.println(sortNumber+"个数据运行1次的时间:");
    			Integer[] list = new Integer[sortNumber];
    			//将数据存到数组中
    			for(int i=0;i<sortNumber;i++){
    				list[i] = random.nextInt(sortNumber);
    			}
    			startTime = System.nanoTime();
    			quickSort(list);
    			endTime = System.nanoTime();
    			time = endTime - startTime;
    			System.out.println("快速排序:"+time+"纳秒");
    		}
    	}
    }
```
  
** 然后是实验结果和分析： **

![](http://img.blog.csdn.net/20150325090414145?watermark/2/text/aHR0cDovL2Jsb2
cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/di
ssolve/70/gravity/Center) ![](http://img.blog.csdn.net/20150325090523457?water
mark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsiz
e/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) ![](http://img.blog.csdn.n
et/20150325090537045?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0
ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) 
![](http://img.blog.csdn.net/20150325090548058?watermark/2/text/aHR0cDovL2Jsb2
cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/di
ssolve/70/gravity/Center) ![](http://img.blog.csdn.net/20150325090631581?water
mark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsiz
e/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) ![](http://img.blog.csdn.n
et/20150325090609399?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0
ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) 
![](http://img.blog.csdn.net/20150325090639180?watermark/2/text/aHR0cDovL2Jsb2
cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/di
ssolve/70/gravity/Center) ![](http://img.blog.csdn.net/20150325090732515?water
mark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsiz
e/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) ![](http://img.blog.csdn.n
et/20150325090746461?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0
ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) 
![](http://img.blog.csdn.net/20150325090759175?watermark/2/text/aHR0cDovL2Jsb2
cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/di
ssolve/70/gravity/Center) ![](http://img.blog.csdn.net/20150325090735979?water
mark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsiz
e/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) ![](http://img.blog.csdn.n
et/20150325090819627?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0
ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center) 
![](http://img.blog.csdn.net/20150325090830656?watermark/2/text/aHR0cDovL2Jsb2
cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/di
ssolve/70/gravity/Center) ![](http://img.blog.csdn.net/20150325090808599?water
mark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvUGhlbml4ZmF0ZQ==/font/5a6L5L2T/fontsiz
e/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

通过分析图标信息可知：

10  个数据时：堆排序所花费时间想起其他排序算法时间效率非常低，除此之外其他相差不大

100  个数据时：插入、归并、快速排序最好，冒泡、堆排序最差

1000  个数据时：冒泡排序最差，其他算法相对差距较小

1  万个数据时：冒泡最差，插入、选择较差，归并，堆，快速排序最好

10  万个数据时：冒泡的效率极其之低，插入，选择也很不咋样，剩下的相对都蛮好

100  万个数据时：冒泡、插入、选择已经完全没法用了，而另外三个中，快排最好，接着归并，然后是堆排序

1000  万个数据时，同  100  万个时是一样的

由以上数据分析可得出：

冒泡排序：只有在数据很小，由数据中可看出，在  10  个数据以下时，可以考虑使用

插入排序和选择排序，在数据个数  1000  以下时，还是可以采用的，当数据达到  1  万时，效率已经远远不如某些其他算法了

在数据大于  1  万以上后，我们可以发现，冒泡，插入，选择排序这些已经不能看了，而在剩下的中，最好的是快速，接着是归并，然后是堆排序

  

所以，在不考虑空间复杂度的情况下，对排序算法的选择有以下建议：

数据个数很小时（不足  100  ），不要使用堆排序

当数据大于  100  后，就不要在使用冒泡了，选择其他的吧

当数据达到  1  万以上，数据个数已经很大后，建议使用快速排序

给懒人最好的建议，别想太多，直接用快速排序就好，虽然快速排序有些情况不如别的排序算法，但是差距不会很大，而当数据数量很大时，快排就是最好的选择

  

