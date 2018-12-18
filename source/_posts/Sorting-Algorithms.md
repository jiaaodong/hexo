---
title: Sorting Algorithms
date: 2018-08-19 13:41:47
tags:
---

These days I am reading the book <**Introduction to algorithms**>. This week I have learnt several sorting algorithms in this book and implemented some of them. The mentioned algorithms are merge sort, heap sort, quick sort, counting sort, radix sort and bucket sort. During learning, I also learnt some methods to analyze and design an algorithm, among which the most important one is divide-and-conquer.

## Merge Sort ## 
Merge sort is an algorithm that divide the whole sort task into two subtasks. Then divide each subtask also into two subtasks. Until the list contains only one element, it will be merged into larger lists with ordered elements. The design principle is **divide-and-conquer**. 

The python code of merge sort is:  
```
# Merge two sorted series
import numpy as np
import math

def Merge(A, p, q, r):
    n1 = q - p + 1
    n2 = r - q
    L = np.zeros(n1+1)
    R = np.zeros(n2+1)
    for i in np.arange(0, n1):
        L[i] = A[p + i]
    for j in np.range(0, n2):
        R[j] = A[q + j + 1]
    L[n1] = math.inf
    R[n2] = math.inf
    i = 0
    j = 0
    for k in np.arange(p, r+1):
        if L[i] < R[j]:
            A[k] = L[i]
            i = i + 1
        else:
            A[k] = R[j]
            j = j + 1

def Merge_Sort(A, p, r):
    if p < r:
        q = int(np.floor((p + r) / 2))
        Merge_Sort(A, p, q)
        Merge_Sort(A, q+1, r)
        Merge(A, p, q, r)

A = [4., 1., 7., 2., 3., 5., 9, 4]
print("The original array A: ", A)
# Merge(A, 0, 0, 1)
# B = Merge(A, 0, 2, 5)
Merge_Sort(A, 0, len(A)-1)
print("The array A after merge-sort: ", A)

## Pay special attention to the integer value for the np.array(shape)

```
## Heap Sort ##
Let's start with insertion sort. Insertion sort is making many repeated comparisons. Although it is better than randomly choosing a pair of elements and comparing all possible pairs($n!$ times comparison). It still does many unnecessary comparings. For example, if $A$ is already larger than $B$ and $B$ is already larger than $C$, it is useless to compare $A$ and $C$. From the proof in the book, we could see that the most efficient way to do the comparing should take at least $O(nlog(n))$ time. Heap sort has achieved this bound. 

- Heap sort uses a data structure called **Heap** to facilitate the sorting. 
- It uses the hierarchical structure of the tree to implement a more efficient comparison than insertion sort. 
- The heap is basically a list,  that can be found in most programming languages. 
- The heap is manipulating the index of the list. Most data in this heap(about $n/2$) is located on the leaves. 
- This sorting algorithm is **in place**.  It only does exchanges. 
- A heap not only contains the value and index of a list. It also contains a value called **heap size**. Therefore, the algorithm is implemented by a class. 
- **Max_heapify** is a procedure where the lower element flows downwards.
- Python can do the exchange in `A[largest], A[i] = A[i], A[largest]`
- Why we need to repeat heapify buttom-up when building a max heap? Because the heapify algorithm couldn't 'see' more than 2 levels of the binary tree. In other words, it is uncertain whether the subtree of the selected node is a max heap.
- The index conventions are different over different programming languages. If the first index of a list is $0$, the left node is $ 2i+1 $ and the right node is $ 2i + 2 $ . 

The code of heap sort is shown bellow:

```
class sort:
    """ This is a class of sort algorithms"""
    def __init__(self, array):
       self.A = array
       self.HeapSize = len(B)
    
    def max_heapify(self, i):
        largest = i 
        l = 2 * i + 1
        r = 2 * i + 2
        if l < self.HeapSize and self.A[l] > self.A[i]:
                largest = l
        if r < self.HeapSize and self.A[r] > self.A[largest]:
                largest = r
        if largest != i:
            self.A[i], self.A[largest] = self.A[largest], self.A[i]
            self.max_heapify(largest)
            
    def build_max_heap(self):
        self.HeapSize = len(self.A)
        for i in range(len(self.A) // 2, 0, -1):
            self.max_heapify(i)
            
    def heap_sort(self):
        self.build_max_heap()
        for i in range(len(self.A)-1, 0, -1):
            self.A[0], self.A[i] = self.A[i], self.A[0]
            self.HeapSize = self.HeapSize - 1
            self.max_heapify(0)
            
    
B = [16, 4, 10, 14, 7, 9, 3, 2, 8, 1]
HeapSort = sort(B)
HeapSort.heap_sort()
HeapSort.A
```
## Quick Sort ##
Quick sort is achieved by a method called **partitioning**. This algorithm is in worst case $O(n^2)$ but on average $O(nlog(n))$. It uses the principle of randomized algorithms, which induce some degree of random into the algorithm.  

The python code:
```
import numpy as np
class quicksort():
    def __init__(self, array):
        self.A = array


    def partition(self, p, r):
        x = self.A[r]
        i = p - 1
        for j in np.arange(p, r):
            if self.A[j] <= x:
                i += 1
                self.A[i], self.A[j] = self.A[j], self.A[i]
        self.A[i + 1], self.A[r] = self.A[r], self.A[i + 1]
        return i + 1

    def randomized_partition(self, p, r):
        i = np.random.randint(p, r + 1) # [p, r] range
        self.A[i], self.A[r] = self.A[r], self.A[i]
        return self.partition(p, r)

    def randomized_quicksort(self, p, r):
        if p < r:
            q = self.randomized_partition(p, r)
            self.randomized_quicksort(p, q - 1)
            self.randomized_quicksort(q+1, r)
#__________________________________________________________
test_array = [13, 19, 9, 5, 12, 8, 7, 4, 21, 2, 6, 11]
print('Input array:', test_array)
sort_task = quicksort(test_array)
sort_task.randomized_quicksort(0, len(test_array) - 1)
print('Output array: ', test_array)
```

## Counting Sort, Radix Sort and Bucket Sort ##
They are not limited by the bound of $ \Omega(n*log(n)) $ because they depend on some requirements or some calculations. 
### Counting Sort ###
- Elements value start from zero and no more than $w$. All elements are integers.



