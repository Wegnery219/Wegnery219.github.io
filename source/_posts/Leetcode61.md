---
title: Leetcode61:Rotate List
tags: code
comment: true
date: 2019-09-22 12:09:00
---
### 题目描述
Given a linked list, rotate the list to the right by k places, where k is non-negative.
Example 1:
Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL
Example 2:
Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL
### 解
第一种想法：
```
1->2->3->4->5 k=2
1->2->3 4->5 分别将前后两个反转一下
3->2->1->5->4 然后再整体反转
4->5->1->2->3
```
就是两个叠在一起的东西，想让下面的变到上面去，就分别转一下然后整体转一下就可以了
#### python code[24ms 11.8MB]
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def reversek(self, headp, head, k):
        before = None
        after = None
        rt = None
        for i in range(k):
            if i==k-1:
                headp.next.next = head.next
                if before:
                    head.next=before
                rt = headp.next
                headp.next = head
            elif i==0:
                before = head
                head = head.next
            else:
                after = head.next
                head.next = before
                before = head
                head = after
        return rt
    def rotateRight(self, head, k):
        """
        :type head: ListNode
        :type k: int
        :rtype: ListNode
        """
        headp = ListNode(0)
        headp.next = head
        copy = ListNode(0)
        copy = head
        length=0
        while copy:
            copy = copy.next
            length = length + 1
        if length==0:
            return head
        if k>=length:
            k = k%length
        if k==0:
            return head
        newheadp=self.reversek(headp,headp.next,length-k)
        newheadp = self.reversek(newheadp,newheadp.next,k)
        newheadp = self.reversek(headp,headp.next,length)
        return headp.next
```
WA1:没有考虑k>length的情况
WA2:没有考虑空链表
WA3:在reversek中，没有考虑k-1==0的情况，if和elif有冲突
第二种想法：
看的讨论区，是比如把1->2->3变成1->2->3->1->2->3->...这样就可以直接从中间截取一段。妙啊
#### c++ code[8ms 9MB]
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
    ListNode* rotateRight(ListNode* head, int k) {
        if(!head) return head;
        ListNode* copy = head;
        int length = 1;
        while (copy->next)
        {
            length++;
            copy = copy->next;
        }
        copy->next = head;
        ListNode* slow = head;
        k = k%length;
        for (int i = 0; i < (length-k); i++)
        {
            copy = copy->next;
            slow = slow->next;
        }
        copy->next=NULL;
        return slow;
    }
};
```
第三种想法：
第三种想法是用一个存储结构来存链表的指针，因为这个rotate只需要交换几个指针就可以了。
#### c++ code[12ms 9.7MB]
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
    ListNode* rotateRight(ListNode* head, int k) {
        if (!head) return head;
        ListNode* copy = head;
        unordered_map<int, ListNode*> m;
        int length=1;
        while (copy->next)
        {
            pair<int, ListNode*> tmp {length, copy};
            m.insert(tmp);
            length++;
            copy = copy->next;
        }
        pair<int,ListNode*> tmp {length, copy};
        m.insert(tmp);
        k = k%length;
        int l = length - k;
        if(k==0) return head;
        m.at(l)->next=NULL;
        m.at(length)->next=m.at(1);
        return m.at(l+1);
    }
};
```
WA1: 没有考虑k=0的情况
