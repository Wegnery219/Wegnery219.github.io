---
title: Leetcode25:Reverse Nodes in k-Group
tags: code
comment: true
date: 2019-09-21 12:09:00
---
### 题目描述
Given a linked list, reverse the nodes of a linked list k at a time and return its modified list.
k is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of k then left-out nodes in the end should remain as it is.
Example:
Given this linked list: 1->2->3->4->5
For k = 2, you should return: 2->1->4->3->5
For k = 3, you should return: 3->2->1->4->5
Note:
Only constant extra memory is allowed.
You may not alter the values in the list's nodes, only nodes itself may be changed.
### 解
先写一个reversek的函数，然后调用length//k次这个函数
#### python code[32ms 13.1MB]
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reversek(self, headp, head,k):
        before = ListNode(0)
        after = ListNode(0)
        rt = ListNode(0)
        for i in range(k):
            if head==None:
                return None
            if i==0:
                before = head
                head = head.next
            elif i==k-1:
                headp.next.next=head.next
                rt = headp.next
                head.next=before
                headp.next=head
            else:
                after = head.next
                head.next=before
                before = head
                head = after
        return rt
    def reverseKGroup(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        if k==1:
            return head
        headpointer = ListNode(0)
        headpointer.next = head
        headpp = ListNode(0)
        headpp.next=headpointer
        copy = ListNode(0)
        copy = head
        l=0
        while copy:
            copy = copy.next
            l=l+1
        i = l//k
        while i:
            headpointer = self.reversek(headpointer,headpointer.next,k)
            i = i - 1
        return headpp.next.next
```
#### c++ code[20ms 9.6MB]
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
    ListNode* reversek(ListNode* headp, ListNode* head, int k){
        ListNode* after = new ListNode(0);
        ListNode* before = new ListNode(0);
        ListNode* rt = new ListNode(0);
        for(int i=0;i<k;i++){
            if (i==0)before=head,head=head->next;
            else if (i==k-1)
            {
                rt=headp->next;
                headp->next->next=head->next;
                headp->next=head;
                head->next=before;
            }
            else{
                after = head->next;
                head->next = before;
                before = head;
                head = after;
            }
        }
        return rt;
    }
    ListNode* reverseKGroup(ListNode* head, int k) {
        if(k==1) return head;
        ListNode* headpp = new ListNode(0);
        ListNode* headp = new ListNode(0);
        headpp->next=headp;
        headp->next=head;
        ListNode* copya=head;
        int length=0;
        while (copya)
        {
            copya = copya->next;
            length++;
        }
        int times = length/k;
        while (times!=0)
        {
            headp = reversek(headp,headp->next,k);
            times--;
        }
        return headpp->next->next;
    }
};
```
WA1:没考虑k=1的情况
