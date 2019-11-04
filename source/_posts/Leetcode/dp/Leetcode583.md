---
title: 583. Delete Operation for Two Strings
tags: code
comment: true
date: 2019-11-4 12:09:00
---
今天本来应该做Edit Distance的，但是我看了十分钟之后发现想法有点混乱，就从similar question里面挑了一个medium做，明天再看看另一个。
### 题目描述
Given two words word1 and word2, find the minimum number of steps required to make word1 and word2 the same, where in each step you can delete one character in either string.
Example 1:
Input: "sea", "eat"
Output: 2
Explanation: You need one step to make "sea" to "ea" and another step to make "eat" to "ea".
Note:
The length of given words won't exceed 500.
Characters in given words can only be lower-case letters.
### 解
利用了最长公共子字符串，最后修改后留下的same word一定是LCS，证明可以用反证，假设最后留下的same word不是LCS,说明它比LCS要短，那么每个word做的删除次数就要比最后same word是LCS的情况要多，所以就不是minimum steps。而LCS可以用动态规划解决，转移公式：
```
dp[i][j]=dp[i-1][j-1]+1 if dp[i]==dp[j]
dp[i][j]=max(dp[i-1][j],dp[i][j-1]) if dp[i]!=dp[j]
```
忘记原因了可以去翻翻算法导论第15章
#### c++ code[32ms 14.5MB]
```
class Solution {
public:
    int minDistance(string word1, string word2) {
        int l1=word1.size();
        int l2=word2.size();

        vector<int> row(l2+1,0);
        vector<vector<int>> dp(l1+1, row);

        for(int i=1;i<=l1;i++){
            for(int j=1;j<=l2;j++){
                if(word1[i-1]==word2[j-1]) dp[i][j]=dp[i-1][j-1]+1;
                else dp[i][j]=max(dp[i-1][j],dp[i][j-1]);
            }
        }

        return l1+l2-2*dp[l1][l2];
    }
};
```
#### python code[296ms 16MB]
```
class Solution:
    def minDistance(self, word1: str, word2: str) -> int:
        l1=len(word1)
        l2=len(word2)

        dp=[[0 for _ in range(l2+1)] for _ in range(l1+1)]

        for i in range(1,l1+1):
            for j in range(1,l2+1):
                if word1[i-1]==word2[j-1]:
                    dp[i][j] = dp[i-1][j-1]+1
                else:
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1])
        
        return l1+l2-2*dp[l1][l2]
```
之前在做[LIS]()的时候就想过这里的二维数组可不可以简化成一维，当时没有想出来，是因为总想着用一个数来代替dp[i-1][j-1]，这样在j继续往前遍历的时候又会丢掉后面的，看了今天的讨论区，正确做法应该是用一个temp数组来代替，这样最后空间复杂度为O(2*N)
#### c++ code[16ms 9.1MB]
```
class Solution {
public:
    int minDistance(string word1, string word2) {
        int l1=word1.size();
        int l2=word2.size();

        vector<int> dp(l2+1, 0);
        vector<int> temp(l2+1,0);

        for(int i=1;i<=l1;i++){
            for(int j=1;j<=l2;j++){
                if(word1[i-1]==word2[j-1]) dp[j]=temp[j-1]+1;
                else dp[j]=max(temp[j],dp[j-1]);
            }
            temp.assign(dp.begin(),dp.end());
        }

        return l1+l2-2*dp[l2];
    }
};
```
数组的长度可以取max(l1,l2)
