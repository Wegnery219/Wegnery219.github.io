---
title: Leetcode 1028:Recover a Tree From Preorder Traversal
tags: code
comment: true
date: 2019-10-21 12:09:00
---
### 题目描述
We run a preorder depth first search on the root of a binary tree.
At each node in this traversal, we output D dashes (where D is the depth of this node), then we output the value of this node.  (If the depth of a node is D, the depth of its immediate child is D+1.  The depth of the root node is 0.)
If a node has only one child, that child is guaranteed to be the left child.
Given the output S of this traversal, recover the tree and return its root.
Example 1:
![](https://assets.leetcode.com/uploads/2019/04/08/recover-a-tree-from-preorder-traversal.png)
Input: "1-2--3--4-5--6--7"
Output: [1,2,5,3,4,6,7]
Example 2:
![](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114101-pm.png)
Input: "1-2--3---4-5--6---7"
Output: [1,2,5,3,null,6,null,4,null,7]
Example 3:
![](https://assets.leetcode.com/uploads/2019/04/11/screen-shot-2019-04-10-at-114955-pm.png)
Input: "1-401--349---90--88"
Output: [1,401,null,349,88,90]
Note:
The number of nodes in the original tree is between 1 and 1000.
Each node will have a value between 1 and 10^9.
[link:https://leetcode.com/problems/recover-a-tree-from-preorder-traversal/](https://leetcode.com/problems/recover-a-tree-from-preorder-traversal/)
### 解
根据‘-’的数量拆成左右子树递归
#### python code[220ms 14.4MB]
```
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def convert(self, value: List[int]) -> int:
        sum=0
        length = len(value)
        for val in value:
            sum += val * 10**(length-1)
            length -= 1
        return sum

    def helper(self, S: str, left: int, right: int) -> TreeNode:
        # judgement
        if right<left:
            return None
        # get root value
        value=[]
        i = left
        while i<=right and S[i].isdigit():
            value.append(int(S[i]))
            i = i + 1
        root_val = self.convert(value)
        # construct tree
        root = TreeNode(root_val)
        layer = 0
        while i<=right and S[i]=='-':
            layer += 1
            i += 1
        left_begin=i
        left_end=right
        while i<=right:
            if S[i].isdigit():
                i += 1
            else:
                count=0
                left_end = i-1
                while i<=right and S[i]=='-':
                    count += 1
                    i += 1
                if count==layer:
                    break
                else:
                    left_end = right
        root.left = self.helper(S, left_begin, left_end)
        root.right = self.helper(S, i, right)
        return root

    def recoverFromPreorder(self, S: str) -> TreeNode:
        return self.helper(S, 0, len(S)-1)
```
WA1:没有加else: left_end=right,会导致如果找了一次没找到右边开始的起点的话左边的终点位置是错的。
#### c++ code[96ms 363.3MB]
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
    int convert(vector<int> &value){
        int sum=0;
        int length=value.size();
        for(auto val:value){
            sum += val*pow(10,length-1);
            length--;
        }
        return sum;
    }
    TreeNode* helper(string S, int left, int right){
        //judgement
        if(right<left) return nullptr;

        //get root value
        vector<int> value;
        int i = left;
        while(i<=right && isdigit(S[i])){
            //cout<<S[i]<<' ';
            value.push_back(S[i]-'0');
            i = i + 1;
        }
        cout<<endl;
        int root_val = convert(value);

        //construct tree
        TreeNode* root = new TreeNode(root_val);
        int layer=0;
        while (i<=right && S[i]=='-'){
            layer += 1;
            i += 1;
        }
        int left_begin = i;
        int left_end = right;
        while (i<=right){
            if(isdigit(S[i])) i++;
            else{
                int count=0;
                left_end=i-1;
                while (i<=right && S[i]=='-')
                {
                    count++;
                    i++;
                }
                if (count==layer) break;
                else left_end=right;
            }
        }
        root->left = helper(S, left_begin, left_end);
        root->right = helper(S, i, right);
        return root;
    }
    TreeNode* recoverFromPreorder(string S) {
        return helper(S,0,S.size()-1);
    }
};
```
WA1: int(S[i])不是将char变成int，要用S[i]-'0'。
WA2: 在while循环的时候使用left_end又用int重定义了，导致出了循环之后left_end的值不对。
