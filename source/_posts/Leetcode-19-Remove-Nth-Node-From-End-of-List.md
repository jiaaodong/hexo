---
title: Leetcode-19 Remove Nth Node From End of List
date: 2018-12-25 07:15:41
tags: Leetcode
---

# 题目：
Given a linked list, remove the n-th node from the end of list and return its head.
# 例子：
Given linked list: 1->2->3->4->5, and n = 2.

After removing the second node from the end, the linked list becomes 1->2->3->5.

# 注释：
Given n will always be valid.

# 额外要求：
Could you do this in one pass?

# 思路：
根据提示，可以用两个指针，`first`指针标记链表的末尾，`second`指针标记链表末尾之前n位的链表。然后当链表到了末尾后，将`second`指针的下一位连到`second`指针的上一位。

# 知识点：
- `second`指针要在`first`指针之前的`n+1`位，因为这是单链表，没有`second->prev`的指针，所以在去除链表倒数第`n`位的时候，只能用`second->next = second->next->next`。
- 所有的临界情况都要考虑全面，比如只有一个元素的时候`input: [1] 1`
- 出于上面一条的原因，`first`和`second`指针的起点要设置在`head`之前，可以省去判断。
- C++的指针要用`->`运算符
- 复习C++的constructor：如果需要返回的是一个指针，要用`ListNode* first = new ListNode(0)`
- 函数的输入参数中，链表头是`head`指针，指向的是一个数值为`val`，下一位是`next`的`ListNode`，如果被去掉的是链表头，那`head`依然还是会指向这个node。因为我们是用`second->next = second->next->next`来去掉节点的，所以改变的是`second->next`，没有改变`head`，即使`second->next`是从`head`开始的，但`second`只是个指针变量，存储的是当前指向的地址。
- 出于上面一条的原因，可以用一个`dummy`变量存储初始点的地址，最后返回`dummy->next`

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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* ptr1 = new ListNode(0);
        ListNode* ptr2 = new ListNode(0);
        ListNode* dummy = ptr1;
        ptr1->next = head;
        ptr2->next = head;
        for(int i =0; i<n; i++){
            ptr2 = ptr2->next;
        }
        while(ptr2->next!=NULL){
            ptr2 = ptr2->next;
            ptr1 = ptr1->next;
        }
        ptr1->next = ptr1->next->next;
        
        return dummy->next;
    }
     
};
```