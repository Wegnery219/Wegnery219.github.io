---
title: Leetcode19:Remove Nth Node From End of List
tags: code
comment: true
date: 2019-09-18 12:09:00
---
### 题目描述
Given a linked list, remove the n-th node from the end of list and return its head.
Example:
Given linked list: 1->2->3->4->5, and n = 2.
After removing the second node from the end, the linked list becomes 1->2->3->5.
Note:
Given n will always be valid.
Follow up:
Could you do this in one pass?
### 解
链表，主要是边界条件的考虑，避免越界，有几种特殊情况是删最头的，删最末的，和长度为1的数组，每种情况考虑一下，就基本不会报好多错错错错啦。Trick:链表题目可以在开头定义一个指针指向最后需要返回的链表的头，避免因为中间过程的挪动导致最后返回错误。
#### c++ code[4ms 8.6MB]
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
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode headpointer=ListNode(0);
        headpointer.next=head;
        int length = 1;
        while (head->next!=NULL)
        {
            length++;
            head = head->next;
        }
        int counter = length - n;
        head = headpointer.next;
        if (counter==0) headpointer.next = head->next;
        else{
            while (counter>1)
            {
                head = head->next;
                counter--;
            }
            head->next = head->next->next;
        }
        return headpointer.next;
    }
};
```
#### python code[20ms 11.9MB]
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def removeNthFromEnd(self, head, n):
        """
        :type head: ListNode
        :type n: int
        :rtype: ListNode
        """
        headpointer = ListNode(0)
        headpointer.next = head
        length = 1
        while head.next:
            length += 1
            head = head.next
        counter = length - n
        head = headpointer.next
        if counter==0:
            headpointer.next = head.next
        else:
            while counter>1:
                head = head.next
                counter -= 1
            head.next = head.next.next
        return headpointer.next
```
