---
title: Leetcode106:Construct Binary Tree from Inorder and Postorder Traversal
tags: code
comment: true
date: 2019-10-19 12:09:00
---
### 题目描述
Given inorder and postorder traversal of a tree, construct the binary tree.
Note:
You may assume that duplicates do not exist in the tree.
For example, given
inorder = [9,3,15,20,7]
postorder = [9,15,7,20,3]
Return the following binary tree:
```
    3
   / \
  9  20
    /  \
   15   7
```
### 解
#### python code[408ms 15.7MB]
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def helper(self, inorder, postorder, inl, inr, pol, por):
        # judgement
        if inr<inl:
            return None
        root = TreeNode(postorder[por])
        for i in range(len(inorder)):
            if inorder[i]==postorder[por]:
                break
        root.right = self.helper(inorder, postorder, i+1, inr, por-inr+i, por-1)
        root.left = self.helper(inorder, postorder, inl, i-1, pol, por-inr+i-1)
        return root

    def buildTree(self, inorder, postorder):
        """
        :type inorder: List[int]
        :type postorder: List[int]
        :rtype: TreeNode
        """
        return self.helper(inorder, postorder, 0, len(inorder)-1, 0, len(postorder)-1)
        
```
#### c++ code[16ms 17.4MB]
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
    TreeNode* helper(vector<int>& inorder, vector<int>& postorder, unordered_map<int,int>& m,int inl, int inr, int pol, int por){
        if(inr<inl) return nullptr;
        TreeNode* root = new TreeNode(postorder[por]);
        int i = m[postorder[por]];
        root->right = helper(inorder, postorder, m, i+1, inr, por-inr+i, por-1);
        root->left = helper(inorder, postorder, m, inl, i-1, pol, por-inr+i-1);
        return root;
    }
    TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder) {
        unordered_map<int,int> m;
        for(int i=0;i<inorder.size();i++){
            m[inorder[i]]=i;
        }
        return helper(inorder, postorder, m, 0, inorder.size()-1, 0, postorder.size()-1);
    }
};


```
