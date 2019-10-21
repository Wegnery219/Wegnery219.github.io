---
title: Leetcode112&113:Path Sum I&II
tags: code
comment: true
date: 2019-09-30 12:09:00
---
## Leetcode 112 Path Sum
### 题目描述
Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.
Note: A leaf is a node with no children.
Example:
Given the below binary tree and sum = 22,
```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \      \
7    2      1
```
return true, as there exist a root-to-leaf path 5->4->11->2 which sum is 22.
### 解
1. 递归，注意是要递归到叶节点
#### python code[36ms 15.6MB]
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def hasPathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: bool
        """
        if not root:
            return False
        return self.recursive(root, 0, sum)

    def recursive(self, root, now, target):
        now += root.val

        if now == target and not root.left and not root.right:
            return True
        
        if not root.left:
            left = False
        else:
            left = self.recursive(root.left, now, target)
        if not root.right:
            right = False
        else:
            right = self.recursive(root.right, now, target)

        return left or right
```
2. 用栈
#### c++ code[12ms 19.8MB]
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
    bool hasPathSum(TreeNode* root, int sum) {
        stack<TreeNode*> s;
        if(!root) return false;
        s.push(root);
        while (!s.empty())
        {
            TreeNode* tmp = s.top();
            s.pop();
            if(tmp->val==sum && tmp->left==nullptr && tmp->right==nullptr) return true;
            if(tmp->left){
                tmp->left->val += tmp->val;
                s.push(tmp->left);
            }
            if(tmp->right){
                tmp->right->val += tmp->val;
                s.push(tmp->right);
            }
        }
        return false;
    }
};
```
## Leetcode 113 Path Sum II
### 题目描述
Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.
Note: A leaf is a node with no children.
Example:
Given the below binary tree and sum = 22,
```
      5
     / \
    4   8
   /   / \
  11  13  4
 /  \    / \
7    2  5   1
```
Return:
[
   [5,4,11,2],
   [5,8,4,5]
]
### 解
dfs
#### c++ code[12ms 19.8MB]
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
    vector<int> tmp;
public:
    void dfs(vector<vector<int>> &results, TreeNode* root, int target){
        if (root==nullptr) return;
        tmp.push_back(root->val);
        if(root->val==target && root->left==nullptr && root->right==nullptr) results.push_back(tmp);
        else{
            dfs(results, root->left, target-root->val);
            dfs(results, root->right, target-root->val);
        }
        tmp.pop_back();
    }
    vector<vector<int>> pathSum(TreeNode* root, int sum) {
        vector<vector<int>> res;
        tmp.clear();
        dfs(res, root, sum);
        return res;
    }
};
```
#### python code[24ms 14.4MB]
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    tmp=[]
    def dfs(self, results, root, target):
        if root==None:
            return
        self.tmp.append(root.val)
        if root.left==None and root.right==None and root.val==target:
            results.append(self.tmp[:])
        else:
            self.dfs(results, root.left, target-root.val)
            self.dfs(results, root.right, target-root.val)
        self.tmp.pop()

    def pathSum(self, root, sum):
        """
        :type root: TreeNode
        :type sum: int
        :rtype: List[List[int]]
        """
        res = []
        self.dfs(res, root, sum)
        return res
```

WA1:res返回的一直是空数组，之前写错的原因是这一句`results.append(self.tmp)`这一句的时候每次都会append同一个对象，就是self.tmp，每次改动tmp的时候results里的每一个结果也会变。
python如何定义一个n*m的二维数组？

`[[0]*m]*n`的做法是错误的，等价于下面这段代码：

```
arr=[]
tmp=[0]*m
for _ in range(n):
    arr.append(tmp)

In: arr
Out: [[0,0,0],[0,0,0],[0,0,0]]

In: arr[0][0]=5
Out: [[5,0,0],[5,0,0],[5,0,0]]
```
每一个存进去的都是tmp的引用，改变一个tmp的值，其他的都会改变。
正确定义方法：[[0]*m for _ in range(n)]
等价于：
```
arr=[]
for _ in range(n):
    arr.append([0]*m)

In: arr
Out: [[0,0,0],[0,0,0],[0,0,0]]

In: arr[0][0]=5
Out: [[5,0,0],[0,0,0],[0,0,0]]
```
所以results.append的时候相当于每次都把同一个对象的引用传进去，所以一变tmp就全都变了，解决方法是copy一份，python list的简单复制方式是slice.`results.append(self.tmp[:])`
