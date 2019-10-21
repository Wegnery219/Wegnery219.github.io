---
title: Leetcode99:Recover Binary Search Tree
tags: code
comment: true
date: 2019-10-15 12:09:00
---
### 题目描述
Two elements of a binary search tree (BST) are swapped by mistake.
Recover the tree without changing its structure.
Example 1:
Input: [1,3,null,null,2]
```
   1
  /
 3
  \
   2
```
Output: [3,1,null,null,2]
```
   3
  /
 1
  \
   2
```
Example 2:
Input: [3,1,4,null,null,2]
```
  3
 / \
1   4
   /
  2
```
Output: [2,1,4,null,null,3]
```
  2
 / \
1   4
   /
  3
```
Follow up:
A solution using O(n) space is pretty straight forward.
Could you devise a constant space solution?
### 解
和上一个一样，中序遍历的时候产生的数组中找出来交换顺序的两个，然后把两个treenode交换数值即可。
#### python code[64ms 12MB]
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def inOrder(self, root):
        if not root:
            return []
        else:
            return self.inOrder(root.left)+[root]+self.inOrder(root.right)
    def recoverTree(self, root):
        """
        :type root: TreeNode
        :rtype: None Do not return anything, modify root in-place instead.
        """
        inorder = self.inOrder(root)
        wrong_record=[]
        if len(inorder)==0:
            return
        for i in range(0,len(inorder)-1,1):
            if inorder[i].val>inorder[i+1].val:
                wrong_record.append(i)
                break
        for i in range(len(inorder)-1,0,-1):
            if inorder[i].val<inorder[i-1].val:
                wrong_record.append(i)
                break
        tmp = inorder[wrong_record[0]].val
        inorder[wrong_record[0]].val = inorder[wrong_record[1]].val
        inorder[wrong_record[1]].val = tmp
```
#### c++ code[28ms 19.8MB]
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
    vector <TreeNode*> inOrder(TreeNode* root){
        vector<TreeNode*> res;
        if(root==nullptr) return res;
        stack<TreeNode*> s;
        while (root)
        {
            s.push(root);
            root = root->left;
        }
        while (!s.empty())
        {
            TreeNode* now = s.top();
            s.pop();
            res.push_back(now);
            now = now->right;
            while (now)
            {
                s.push(now);
                now = now->left;
            }
        }
        return res;
    }
    void recoverTree(TreeNode* root) {
        vector<TreeNode*> inorder = inOrder(root);
        vector<int> wrong_record;
        for (int i = 0; i < inorder.size()-1; i++)
        {
            if(inorder[i]->val>inorder[i+1]->val){
                wrong_record.push_back(i);
                break;
            }
        }
        for (int i = inorder.size()-1; i >0; i--)
        {
            if(inorder[i]->val<inorder[i-1]->val){
                wrong_record.push_back(i);
                break;
            }
        }
        if(wrong_record.size()!=2) cout<<"number wrong!";
        int tmp = inorder[wrong_record[0]]->val;
        inorder[wrong_record[0]]->val = inorder[wrong_record[1]]->val;
        inorder[wrong_record[1]]->val = tmp;
        return;
    }
};
```
