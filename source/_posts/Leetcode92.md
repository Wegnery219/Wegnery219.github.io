---
title: Leetcode92:Reverse Linked List II
tags: code
comment: true
date: 2019-09-23 12:09:00
---
### 题目描述
Reverse a linked list from position m to n. Do it in one-pass.
Note: 1 ≤ m ≤ n ≤ length of list.
Example:
Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
### 解
对链表的操控，经过m到n区间的时候，就反转链接，逐步往下传。
#### python code[16ms 12MB]
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reverseBetween(self, head, m, n):
        """
        :type head: ListNode
        :type m: int
        :type n: int
        :rtype: ListNode
        """
        if m==n:
            return head
        length = 0
        headpointer = ListNode(0)
        headpointer.next = head
        iterate = headpointer
        dummy = ListNode(0)
        dummy.next = headpointer
        before = None
        after = None
        while iterate.next:
            if length == m:
                headpointer = before
            elif length>m and length<n:
                after = iterate
                iterate = iterate.next
                after.next = before
                before = after
                length = length + 1
                continue
            elif length==n:
                headpointer.next.next = iterate.next
                headpointer.next = iterate
                iterate.next = before
                length = length + 1
                break
            length = length+1
            before = iterate
            iterate = iterate.next
        if length == n:
            headpointer.next.next = iterate.next
            headpointer.next = iterate
            iterate.next=before
        return dummy.next.next
```
WA1:会tle,因为之前length==n的时候，headpointer.next在headpointer.next.next之前定义，最后构成了一个循环链表。
#### c++ code[4ms 8.5MB]
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
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        ListNode* dummy = new ListNode(0);
        ListNode* headpointer = new ListNode(0);
        ListNode* before = new ListNode(0);
        ListNode* after = new ListNode(0);
        ListNode* iterate = new ListNode(0);
        int length = 0;

        dummy->next = headpointer;
        headpointer->next = head;
        iterate = headpointer;
        if (m==n) return head;
        while (iterate->next)
        {
            if(length == m) headpointer=before;
            else if (length>m && length<n)
            {
                after = iterate;
                iterate = iterate->next;
                after->next = before;
                before = after;
                length++;
                continue;
            }
            else if(length==n){
                headpointer->next->next = iterate->next;
                headpointer->next = iterate;
                iterate->next = before;
                length++;
                break;
            }
            length++;
            before = iterate;
            iterate = iterate->next;
        }
        if (length==n)
        {
            headpointer->next->next = iterate->next;
            headpointer->next = iterate;
            iterate->next = before;
        }
        return dummy->next->next;
    }
};
```
然后又tle了:)))))))，这次是因为`after->next = before;iterate = iterate->next;`这两句写反了，又构成了环，after->next就相当于把iterate->next赋值了。调试的方法就是print，把每一步iterate的value打印出来，看哪里出错了。
别人超精简的代码，留个存档，慢慢学吧.
```
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int m, int n) {
        ListNode node(0);
        node.next = head;
        head = &node;

        ListNode *start = nullptr, *end = nullptr, *pre = nullptr;
        for (int i = 0; head; ++i) {
            ListNode *next = head->next;

            if (i == m) {
                start = pre;
                end = head;
            } else if (m < i && i < n) {
                head->next = pre;
            } else if (i == n) {
                head->next = pre;
                start->next = head;
                end->next = next;
            }

            pre = head;
            head = next;
        }

        return node.next;
    }
};
```
