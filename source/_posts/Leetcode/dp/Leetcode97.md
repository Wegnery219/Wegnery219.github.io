---
title: Leetcode 97. Interleaving String
tags: code
comment: true
date: 2019-11-9 12:09:00
---
### 题目描述
Given s1, s2, s3, find whether s3 is formed by the interleaving of s1 and s2.
Example 1:
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbcbcac"
Output: true
Example 2:
Input: s1 = "aabcc", s2 = "dbbca", s3 = "aadbbbaccc"
Output: false
### 解
想到了LCS问题的解法，从s3的最后一个字符开始，因为如果想通过插入获取s3，则s3中的最后一个字符一定在s1或s2中，状态转移公式为：
```
dp[l1][l2] = dp[l1-1][l2] {if s1[l1]==s3[l3] and s2[l2]!=s3[l3]}
           = dp[l1][l2-1] {if s1[l1]!=s3[l3] and s2[l2]==s3[l3]}
           = dp[l1-1][l2] or dp[l1][l2-1] {if s1[l1]==s3[l3] and s2[l2]==s3[l3]}
           = False {if s1[l1]!=s3[l3] and s2[l2]!=s3[l3]} 
```
#### python code[32ms 12.9MB]
```
from typing import List
class Solution:
    def helper(self, record: List, s1:str, s2:str, s3: str, l1: int, l2: int, l3: int) -> bool:
        if record[l1-1][l2-1]!=2:
            return record[l1-1][l2-1]
        if l3==0:
            if l1==1 and l2==1:
                record[l1-1][l2-1]=1
            else:
                record[l1-1][l2-1]=0
            return record[l1-1][l2-1]
        if s3[l3-1]==s2[l2-1] and s3[l3-1]!=s1[l1-1]:
            record[l1-1][l2-1]=self.helper(record, s1, s2, s3, l1, l2-1, l3-1)
        elif s3[l3-1]!=s2[l2-1] and s3[l3-1]==s1[l1-1]:
            record[l1-1][l2-1]=self.helper(record, s1, s2, s3, l1-1, l2, l3-1)
        elif s3[l3-1]==s2[l2-1] and s3[l3-1]==s1[l1-1]:
            record[l1-1][l2-1]=self.helper(record, s1, s2, s3, l1, l2-1, l3-1) or self.helper(record, s1, s2, s3, l1-1, l2, l3-1)
        else:
            record[l1-1][l2-1]=0
        return record[l1-1][l2-1]
    def isInterleave(self, s1: str, s2: str, s3: str) -> bool:
        s1='*'+s1
        s2='*'+s2

        record=[[2 for _ in range(len(s2))] for _ in range(len(s1))]
        return self.helper(record, s1, s2, s3, len(s1), len(s2), len(s3))
```
