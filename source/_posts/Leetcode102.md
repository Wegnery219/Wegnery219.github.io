---
title: Leetcode102:Binary Tree Level Order Traversal
tags: code
comment: true
date: 2019-10-08 12:09:00
---
### 题目描述
Given a binary tree, return the level order traversal of its nodes' values. (ie, from left to right, level by level).
For example:
Given binary tree [3,9,20,null,null,15,7],
```
    3
   / \
  9  20
    /  \
   15   7
```
return its level order traversal as:
[
  [3],
  [9,20],
  [15,7]
]
### 解
使用了队列，因为要求是从左到右
#### python code[16ms 12.3MB]
collections.deque:
是python的队列，也可以当栈使用(🐂)
init: q = deque()
push: 
q.append(xxx)添加一个元素
q.appendleft(xxx)在左边添加一个元素
q.extend(xxx)添加一组元素
q.extendleft(xxx)在左边添加一个元素
pop: 
q.pop() 从尾端弹出，这时候是当栈用
q.popleft() 从头弹出，当队列
其他用法和list类似
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
from collections import deque
class Solution(object):
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []
        res = []
        q = deque()
        q.append(root)
        while len(q)!=0:
            level_count = len(q)
            tmp = []
            for _ in range(level_count):
                node = q[0]
                q.popleft()
                if node.left!=None:
                    q.append(node.left)
                if node.right!=None:
                    q.append(node.right)
                tmp.append(node.val)
            res.append(tmp)
        return res
```
#### c++ code[8ms 13.8MB]
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        queue<TreeNode*> q;
        vector<vector<int>> res;
        if(root==nullptr) return res;
        q.push(root);
        while (!q.empty())
        {
            int level_count = q.size();
            vector<int> tmp;
            for (int i = 0; i < level_count; i++)
            {
                TreeNode* now = q.front();
                q.pop();
                if(now->left) q.push(now->left);
                if(now->right) q.push(now->right);
                tmp.push_back(now->val);
            }
            res.push_back(tmp);
        }
        return res;
    }
};
```
