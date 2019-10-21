---
title: Leetcode108:Convert Sorted Array to Binary Search Tree
tags: code
comment: true
date: 2019-10-20 12:09:00
---
### 题目描述
Given an array where elements are sorted in ascending order,convert it to a height balanced BST.
For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.
Example:
Given the sorted array: [-10,-3,0,5,9],
One possible answer is: [0,-3,9,-10,null,5], which represents the following height balanced BST:
```
      0
     / \
   -3   9
   /   /
 -10  5
```
### 解
#### python code[80ms 16.2MB]
```
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def hepler(self, nums: List[int], left: int, right: int) ->TreeNode:
        length = right - left + 1
        # judgement
        if right<left:
            return None
        mid = length//2+left
        root = TreeNode(nums[mid])
        root.right = self.hepler(nums, mid+1, right)
        root.left = self.hepler(nums, left, mid-1)
        return root

    def sortedArrayToBST(self, nums: List[int]) -> TreeNode:
        return self.hepler(nums,0,len(nums)-1)
```
WA1:`mid=length//2`没有加left，死循环下去了
#### c++ code[12ms 20.9MB]
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
    TreeNode* helper(vector<int>& nums, int left, int right){
        int length = right-left+1;
        //judgement
        if(right<left) return nullptr;
        int mid = length/2+left;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = helper(nums, left, mid-1);
        root->right = helper(nums, mid+1, right);
        return root;
    }
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return helper(nums,0,nums.size()-1);
    }
};
```
