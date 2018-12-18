---
title: '[Learning Algorithms] - Data Structure Intro'
date: 2018-09-17 23:16:48
tags:
---
## 1. What is data structure ##
From the definition in [Wikipedia Data structure](https://en.wikipedia.org/wiki/Data_structure). 

>In [computer science](https://en.wikipedia.org/wiki/Computer_science "Computer science"), a **data structure** is a data organization, management and storage format that enables efficient access and modification.

This is a two-fold definition. On one hand, it defines what is data structure:
>Data structure is a **data format**.

On the other hand, it explains why do we need different data structures:
>Data structure enables efficient access and modification of the data.

Basically, if we only consider data structure itself, it is all about how to store the data.
## 2. Why do we need data structure ##
Now that we have known what data structure is, we need to know why do we need it. We seem to be very comfortable when we are using MATLAB array and Numpy array. They are easy to use and friendly to math representation. Why do we still need the hassle of implementing a Red-Black tree or a graph?

When we are programming, we always need to load, process and store some data (For MATLAB and Python Numpy users, arrays are usually the case). But simply storing these data is far from enough. We also need to access them when needed. How to access them? The answer of this question seems trivial:

>We can access them by index in MATLAB array or by a key in Python's dictionary. 

However, this is an imcomplete answer. Programming language constructs a bridge between data and programmer, through the language's environment such as an interpreter, a compiler, ect. But how can language access data?  The data is stored in the memory with a certain address. Different data types occupy different space (8 bits for uint8, 16 or 32 bits for float, etc). The address is not very easy for a human being to understand intuitively. They look like a combination of numbers and characters. But we need to know the first, second, third number in our vector simply by the index. Otherwise computer doesn't know what is the first number in our vector or what is the correspondance between a key and the value (Also known as satellitte data) even though we have already stored them in the memory. 

Moreover, If you have used MATLAB, you might also observe that it allows you to grow the array when running the program. But it will warn you that this is not very efficient. This is called dynamic allocation of the memory. In C++ this means allocate memory for the data after compilation. The dynamic data allocation also need data structure.

On top of the storage, we also need to retrieve the data. For example, for a list of numbers, we want the last element or the largest one. It is easy for us to search all of them. But if we check the element one by one and compare them with all the other elements, it might not be a good choice. To efficiently achieve the data retrival, data structure helps a lot. I think that's why in the book *Introduction of algorithms* there are more than one chapter to talk about data structure.

Moreover, many real-life cases need data structure to facilitate programming. For example, if we want to undo some editted content in an editor, the undo button should be implemented by a stack, which is based on the Last-in-first-out (LIFO) principle.

In a word, data is data, but a good way to manage the data can help to save time and space. This is not only achieved by algorithms, but also by smart data structures.

## 3. What is the relationship between data structure and programming language? ##

**Please don't confuse the Data Structure Theories with Programming languages!** 

Data Structure does not rely on certain language, but they need to be implemented by languages. On the other way round,  languages are using certain data structures to form new data structures of their own.  

When I started learning data structure, I am already very familiar with MATLAB, C++ and Python. The data in those programming languages are organized in various ways, such as array and cell in MATLAB, list, tuple, dictionary in Python and vector in C++ standard libraries. So I am always wondering the relationships between what we learn as data structure and the data structures in those languages.

During this process, I am also doing some exercises on [Leetcode](https://leetcode.com/), where there are many problems related to data structure. The same problem can be solved using different languages, which helps me decouple data structure theories with specific language implementations.

For example, when we consider the problems using Hash Table, we are usually deal with some searching problems, such as the problem [#1 Two sum](https://leetcode.com/problems/two-sum/description/). We need to search across previously saved results according to the newly encountered ones. During this procedure, we need correspondances, thus Hash Table is a good data structure to facilitate the procedure. It can retrieve the satellite data by the key with low time complexity. In Python, dictionary is implemented by Hash Table. In Java and C++, Hash Map does the same job.   

Unlike Hash Table has its counterpart in various languages. A tree should be constructed by a Class in most Object-oriented languages. A tree is organized by nodes. Each node stores the pointer to the parent node, the pointers to the children nodes and the value of satellite data. In Leetcode tree node, they are called ` root.left `, `root.right` and `root.val`. (Leetcode tree node doesn't have parent node pointer.) Until now you may ask, is it only be able to implement a tree by pointers? What if a language does not have pointer implementation such as Python? This is what I call "decouples the language and data structure". In leetcode, the incoming tree is just a list. What is the "Pointer"? -The indices of the element. The first element in the list is the root of the tree. The second and the third are the children of the root and so on and so forth. We can use the principle and algorithms of a binary tree to facilitate sorting or other tasks. The list is just a notation.

## 4. Conclusion ##

Data Structure is a theory of how to organize the data. The theory doesn't rely on particular programming languages. We can find Data Structure playing roles not only in any of the programming languages, but also in other fields, such as the look-up-table in the compiler. We can facilitate some computations not only by good algorithms such as Divide-and-conquer, but also by smart Data Structures.

## Reference ##
1. [Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2009). *Introduction to algorithms*. MIT press.](https://mitpress.mit.edu/books/introduction-algorithms-third-edition)