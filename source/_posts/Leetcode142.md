---
title: Leetcode142:Linked List Cycle II
tags: code
comment: true
date: 2019-09-25 12:09:00
---
### 题目描述
Given a linked list, return the node where the cycle begins. If there is no cycle, return null.
To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.
Note: Do not modify the linked list.
Example 1:
Input: head = [3,2,0,-4], pos = 1
Output: tail connects to node index 1
Explanation: There is a cycle in the linked list, where tail connects to the second node.
Follow-up:
Can you solve it without using extra space?
### 解
没有想出不用额外内存的方法唉，最后看了discuss和一篇blog,里面有非常详细的证明:[https://blog.csdn.net/willduan1/article/details/50938210](https://blog.csdn.net/willduan1/article/details/50938210)
#### python code[1188ms 18.2MB]
用了一个额外数组
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def detectCycle(self, head):
        """
        :type head: ListNode
        :rtype: ListNode
        """
        storage = []
        while head:
            if head not in storage:
                storage.append(head)
                head = head.next
            else:
                return head
        return None
```
#### c++ code[8ms 9.6MB]
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
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (1)
        {
            if(fast==nullptr||slow==nullptr||fast->next==nullptr) return nullptr;
            fast = fast->next->next;
            slow = slow->next;
            if(fast==slow) break;
        }
        while (head!=slow)
        {
            head=head->next;
            slow=slow->next;
        }
        return head;
    }
};
```
