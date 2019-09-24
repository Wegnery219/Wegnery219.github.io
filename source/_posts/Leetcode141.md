---
title: Leetcode141:Linked List Cycle
tags: code
comment: true
date: 2019-09-24 12:09:00
---
### 题目描述
Given a linked list, determine if it has a cycle in it.
To represent a cycle in the given linked list, we use an integer pos which represents the position (0-indexed) in the linked list where tail connects to. If pos is -1, then there is no cycle in the linked list.
Example 1:
Input: head = [3,2,0,-4], pos = 1
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the second node.
Example 2:
Input: head = [1,2], pos = 0
Output: true
Explanation: There is a cycle in the linked list, where tail connects to the first node.
Example 3:
Input: head = [1], pos = -1
Output: false
Explanation: There is no cycle in the linked list.
Follow up:
Can you solve it using O(1) (i.e. constant) memory?
### 解
第一种想法，存储是O(N),开一个数组存之前遇到过的ListNode
#### python code[1192ms 18.1MB]
```
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def hasCycle(self, head):
        """
        :type head: ListNode
        :rtype: bool
        """
        if not head:
            return False
        storge = []
        flag = False
        while not flag and head:
            if head not in storge:
                storge.append(head)
            else:
                flag = True
            head = head.next
        return flag
```
第二种想法是别人的(上知识产权课的我开始思考我这样有没有侵犯他人的著作权，定义一个跑的快的指针，一个跑得慢的指针，大概思想就是如果有环(圈)的话，跑的快的一定能追上跑的慢的。
#### c++ code[12ms 9.8MB]
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
    bool hasCycle(ListNode *head) {
        if (!head) return false;
        ListNode* fast = head;
        ListNode* slow = head;
        
        while (1)
        {
            if(fast==nullptr or slow==nullptr or fast->next==nullptr) return false;
            slow = slow->next;
            fast = fast->next->next;
            if (fast==slow) return true;
        }
    }
};
```
WA1:没有考虑head是空
WA2:没有考虑fast->next==nullptr
