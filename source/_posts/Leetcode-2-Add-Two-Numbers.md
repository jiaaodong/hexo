---
title: Leetcode 2. Add Two Numbers
date: 2018-12-19 21:38:38
tags: Leetcode 
---

# 题目要求
You are given two non-empty linked lists representing two non-negative integers. The digits are stored in reverse order and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.

You may assume the two numbers do not contain any leading zero, except the number 0 itself.
# 例子：
>Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.

# 思路：
每一位相加，当前位推入链表，记录进位数值，每一次的当前位都等于两个当前位数字相加再加上进位数字。
# 要点：
- 复习一下c++中地址的使用、类的定义，链表是最简单的常用自定义数据类型之一了。
- 因为是单链表，没有`prev`，所以既要定义起始node的地址，也要记录node的变量名，node的地址在迭代过程中不断在变，为了最后还能找到起始点。
- 比较特殊的情况就是两个数字的位数不同，这种情况下可以在该位补0。
# 代码：
```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode StartFrom(0);
        ListNode *p = &StartFrom;
        int carry=0;
        while (l1 || l2 || carry)
        {
            int term1 = l1 ? l1->val : 0;
            int term2 = l2 ? l2->val : 0;
            int sum = term1 + term2 + carry;
            carry = sum / 10;
            int digits = sum % 10;           
            p->next = new ListNode(digits);
             p = p->next;
            l1 = l1? l1->next : 0;
            l2 = l2? l2->next : 0;
        }
        return StartFrom.next;
    }
};
```
