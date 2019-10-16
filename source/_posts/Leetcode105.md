---
title: Leetcode105:Construct Binary Tree from Preorder and Inorder Traversal
tags: code
comment: true
date: 2019-10-16 12:09:00
---
### 题目描述
Given preorder and inorder traversal of a tree, construct the binary tree.

Note:
You may assume that duplicates do not exist in the tree.
For example, given
preorder = [3,9,20,15,7]
inorder = [9,3,15,20,7]
Return the following binary tree:
```
    3
   / \
  9  20
    /  \
   15   7
```
### 解
根据preorder的第一个元素将inorder中的划分成左边和右边，然后递归。
#### python code[440ms 86.2MB]
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if len(preorder)!=len(inorder) or len(preorder)==0:
            return None
        if len(preorder)==1:
            return TreeNode(preorder[0])
        root = TreeNode(preorder[0])
        for i in range(len(inorder)):
            if inorder[i]==preorder[0]:
                break
        root.left = self.buildTree(preorder[1:1+i],inorder[:i])
        root.right = self.buildTree(preorder[i+1:],inorder[i+1:])
        return root
```
slice操作会每次复制数组，导致耗时很大。
#### c++ code[20ms 16.6MB]
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
    TreeNode* buildtree(vector<int>& preorder, vector<int>& inorder, int pb, int pe, int ib, int ie){
        if(pb>pe) return nullptr;
        else{
            int split=ib;
            int val=preorder[pb];
            int leftsize=0;
            for(;split<=ie;split++,leftsize++){
                if(inorder[split]==preorder[pb]) {break;}
            }
            TreeNode* root = new TreeNode(val);
            root->left = buildtree(preorder, inorder, pb+1, pb+leftsize, ib, split-1);
            root->right = buildtree(preorder, inorder, pb+leftsize+1, pe, split+1, ie);
            return root;
        }
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        return buildtree(preorder, inorder, 0, preorder.size()-1, 0, inorder.size()-1);
    }
};
```
不写代码会经常数组越界
#### c++ code[16ms 17.7MB]
用hash表来存位置，更快，更准，更强！
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
    TreeNode* buildtree(vector<int>& preorder, vector<int>& inorder, unordered_map<int,int>& pos,int pb, int pe, int ib, int ie){
        if(pb>pe) return nullptr;
        else{
            int val=preorder[pb];
            int split = pos[val];
            int leftsize = split-ib;
            TreeNode* root = new TreeNode(val);
            root->left = buildtree(preorder, inorder, pos,pb+1, pb+leftsize, ib, split-1);
            root->right = buildtree(preorder, inorder, pos,pb+leftsize+1, pe, split+1, ie);
            return root;
        }
    }
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        unordered_map<int,int> pos;
        for(int i=0;i<inorder.size();i++){
            pos[inorder[i]]=i;
        }
        return buildtree(preorder, inorder, pos, 0, preorder.size()-1, 0, inorder.size()-1);
    }
};
```
