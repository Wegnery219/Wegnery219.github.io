---
title: Leetcode24:swap nodes in pairs
tags: code
comment: true
date: 2019-09-20 12:09:00
---
### 题目描述
Given a linked list, swap every two adjacent nodes and return its head.
You may not modify the values in the list's nodes, only nodes itself may be changed.
Example:
Given 1->2->3->4, you should return the list as 2->1->4->3.
### 解
就是交换指针的next。
#### python code[16ms 11.9MB]
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def swapPairs(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        a = ListNode(0)
        a.next=head
        headpointer = ListNode(0)
        headpointer.next=a
        while a.next and a.next.next:
            b = a.next
            c = a.next.next
            a.next=c
            b.next=c.next
            c.next=b
            a = a.next.next

        return headpointer.next.next
```
#### c++ code[0ms 8.5MB]
```
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
    ListNode* swapPairs(ListNode* head) {
        ListNode a=ListNode(0);
        a.next = head;
        ListNode *p=&a;
        ListNode headpointer=ListNode(0);
        headpointer.next = p;
        while (p->next && p->next->next)
        {
            ListNode* b = p->next;
            ListNode* c = p->next->next;
            p->next = c;
            b->next = c->next;
            c->next = b;
            p = p->next->next;
        }
        return headpointer.next->next;
    }
};
```
WA1:[1,2,3,4]会返回[1,4,3]，正确应该是[2,1,4,3]，之前定义的是headpointer.next=head,然后return headpointer.next，这样不行的，因为在定义的时候，headpointer就指向了head对应的那块内存，val是1，后面不管指针互相怎么变，那块内存对应的一直是val是1的那个ListNode。
相当于head,headpointer,a都是名字，当时赋值了之后，被赋值的变量就指向了那块地址，后面名字指向的东西变化，但是不会改变那个地址里面的东西。
