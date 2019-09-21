---
title: Leetcode21&23:merge two&k sorted lists
tags: code
comment: true
date: 2019-09-19 12:09:00
---
## Leetcode21:merge two sorted lists
### 题目描述
Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.
Example:
Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4
### 解
双指针，两个指针分别指向两个链表，每次比较大小然后后移。
#### python code[20ms 11.9MB]
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        if not l1:
            return l2
        if not l2:
            return l1
        headpointer = ListNode(0)
        tmp = ListNode(0)
        headpointer.next = tmp
        while l1 and l2:
            v1 = l1.val
            v2 = l2.val
            if v1<=v2:
                tmp.next = l1
                l1 = l1.next
            else:
                tmp.next = l2
                l2 = l2.next
            tmp = tmp.next
        if l1:
            tmp.next=l1
        else:
            tmp.next=l2
        return headpointer.next.next
```
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(!l1) return l2;
        if(!l2) return l1;
        ListNode headpointer = ListNode(0);
        ListNode tmpp = ListNode(0);
        ListNode* tmp = &tmpp;
        headpointer.next=tmp;
        while (l1&&l2)
        {
            int v1 = l1->val;
            int v2 = l2->val;
            if (v1<=v2)
            {
                tmp->next = l1;
                l1 = l1->next;
            }
            else{
                tmp->next = l2;
                l2 = l2->next;
            }
            tmp = tmp->next;
        }
        if(!l1) tmp->next=l2;
        if(!l2) tmp->next=l1;
        return headpointer.next->next;
    }
};
```
## Leetcode23:merge k sorted lists
### 题目描述
Merge k sorted linked lists and return it as one sorted list. Analyze and describe its complexity.
Example:
Input:
[
  1->4->5,
  1->3->4,
  2->6
]
Output: 1->1->2->3->4->4->5->6
### 解
想法就是把每一个链表里面的数提取出来放到一个数组里，然后对数组排序。哈哈哈哈想不到吧！其实正常做法是建一个堆，但是我要明天才能会堆，emm等我会了我会再写一遍的。
#### python code[76ms 20.1MB]
```
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        tmpar=[]
        for li in lists:
            while li:
                tmpar.append(li.val)
                li = li.next
        if len(tmpar)==0:
            return None
        tmpar.sort()
        headpointer = ListNode(0)
        head = ListNode(tmpar[0])
        headpointer.next = head
        for val in tmpar[1:]:
            tmp=ListNode(val)
            head.next=tmp
            head=head.next
        return headpointer.next
```
#### c++ code[24ms 11.7MB]
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
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        std::vector<int> tmp;
        for (auto li: lists) {
            while (li) {
                tmp.push_back(li->val);
                li = li->next;
            }
        }
        std::sort(tmp.begin(), tmp.end());

        ListNode head(0);
        ListNode *p = &head;
        for (auto val: tmp) {
            p->next = new ListNode(val);
            p = p->next;
        }
        return head.next;
    }
};
```
WA1:没有考虑空数组[],[[]]
WA2:临时变量不能取地址`ListNode* p = &ListNode(0)`是不对的，因为临时变量会立即析构，后面指针就指向空了。
WA3:非法访问，没有new，错误信息：`Runtime Error Message:
AddressSanitizer: stack-use-after-scope on address 0x7ffc343d1950 at pc 0x00000042626d bp 0x7ffc343d0660 `,错误代码：
```
for(int i=1;i<tmp.size();i++){
            ListNode p=ListNode(tmp[i]);
            head->next = &p;
            head = head->next;
        }
```
c++执行的时候分栈和堆，函数执行的时候是压栈，创建一个变量就进一个栈，生命周期结束会弹出，栈上变量会自动回收，这里到循环结束，p已经不存在了，所以指针就会指向空。
Q:为啥之前的headpointer定义也是这样的就不会指向空？
A:因为headpointer是到函数结束了才被回收，p是循环结束了就没了。
Q:为啥用new就ok？
A:因为new是申请堆上变量，不会被主动回收，要用delete删除才会被删除掉。new返回一个指针，指向堆上变量。
```
for (auto &li:lists)
        {
            while (li)
            {
                tmp.push_back(li->val);
                li = li->next;
            }
        }
```
这里用引用会改变变量，所以每个li到最后都会指向空。嗯然后lists里就啥也没有了啊哈哈，这里复制是复制指针，一个指针8个字节(64位)，其实也没有复制很多东西啦！
然后下面是idol的堆的code
```
import heapq


class Heap:
    def __init__(self):
        self.sorted = []

    def push(self, item):
        heapq.heappush(self.sorted, item)

    def pop(self):
        return heapq.heappop(self.sorted)

    def __len__(self):
        return len(self.sorted)


class Solution(object):
    def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """
        sorted_nodes = Heap()
        ret = []

        for idx, list_node in enumerate(lists):
            if list_node is not None:
                sorted_nodes.push((list_node.val, idx))

                list_node = list_node.next
                lists[idx] = list_node

        while len(sorted_nodes):
            minimum_v, idx = sorted_nodes.pop()

            ret.append(minimum_v)

            list_node = lists[idx]
            if list_node is not None:
                sorted_nodes.push((list_node.val, idx))

                list_node = list_node.next
                lists[idx] = list_node

        return ret

```
期待周老师堆小讲堂！
