---
title: Leetcode98:Validate Binary Search Tree
tags: code
comment: true
date: 2019-10-9 12:09:00
---
### 题目描述
Given a binary tree, determine if it is a valid binary search tree (BST).
Assume a BST is defined as follows:
The left subtree of a node contains only nodes with keys less than the node's key.
The right subtree of a node contains only nodes with keys greater than the node's key.
Both the left and right subtrees must also be binary search trees.
 Example 1:
```
    2
   / \
  1   3
```
Input: [2,1,3]
Output: true
Example 2:
```
    5
   / \
  1   4
     / \
    3   6
```
Input: [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.
### 解
之前有考虑一种想法，就是两个条件，在原始root左边的所有节点的右节点要小于除了当前root的最小值，就是当前root的爹，同理，右边的左要大于当前root的爹。然后翻译过来是这样的
#### c++ code[WA]
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
    bool isValidBST(TreeNode* root) {
        if(root==nullptr) return true;
        stack<TreeNode* > s;
        s.push(root);
        while (root->left)
        {
            if(root->left->val < root->val){
                s.push(root->left);
                root = root->left;
            }
            else return false;
        }
        while (s.size()!=1)
        {
            TreeNode* now = s.top();
            s.pop();
            if(now->right!=nullptr){
                if(now->right->val >= s.top()->val) return false;
            }
        }
        root = s.top();
        while (root->right)
        {
            if(root->right->val > root->val){
                s.push(root->right);
                root = root->right;
            }
            else return false;
        }
        while (s.size()!=1)
        {
            TreeNode* now = s.top();
            s.pop();
            if(now->left!=nullptr){
                if(now->left->val <= s.top()->val) return false;
            }
        }
        return true;
    }
};
```
错在把左边的右子树当成一个节点了，它可能是个树。
然后我懒得改了。
然后我问idol了。
我有罪，我忏悔，我下次一定不懒(假的
然后就要用到中序遍历了，BST的中序遍历是一个递增数组。
#### c++ code[12ms 20.7MB]
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
    vector<int> inOrder(TreeNode* root){
        vector<int> res;
        if(root==nullptr) return res;
        stack<TreeNode*> s;
        s.push(root);
        while(root->left){
            s.push(root->left);
            root = root->left;
        }
        while (!s.empty())
        {
            TreeNode* now = s.top();
            s.pop();
            res.push_back(now->val);
            if(now->right) {
                s.push(now->right);
                TreeNode* node = now->right;
                while (node->left)
                {
                    s.push(node->left);
                    node = node->left;
                }
            }
        }
        return res;
    }
    bool isValidBST(TreeNode* root) {
        vector<int> inorder = inOrder(root);
        for (int i = 1; i < inorder.size(); i++)
        {
            if(inorder[i]<=inorder[i-1]) return false;
        }
        return true;
    }
};
```
#### python code[52ms 16.8MB]
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def inorder(self, root):
        if not root:
            return []
        else:
            return self.inorder(root.left)+[root.val]+self.inorder(root.right)
    def isValidBST(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """
        inorder = self.inorder(root)
        for i in range(1, len(inorder)):
            if inorder[i]<=inorder[i-1]:
                return False
        return True
```
