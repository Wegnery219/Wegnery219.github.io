---
title: 72. Edit Distance
tags: code
comment: true
date: 2019-11-5 12:09:00
---
### [题目描述](https://leetcode.com/problems/edit-distance/)
Given two words word1 and word2, find the minimum number of operations required to convert word1 to word2.
You have the following 3 operations permitted on a word:
Insert a character
Delete a character
Replace a character
Example 1:
Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')
Example 2:
Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')
### 解
状态转移方法，对于将word1转移到word2
1. insert，在word1中insert相当于在word2中delete，(l1,l2)->(l1,l2-1)+1
2. delete, (l1,l2)->(l1-1,l2)+1
3. replace, (l1,l2)->(l1-1,l2-1)+1
状态转移方程:
```
dp[i][j]=dp[i-1][j-1] if word1[i]==word2[j]
dp[i][j]=min(dp[i-1][j]+1,dp[i][j-1]+1,dp[i-1][j-1]+1) if word1[i]!=word2[j]
```
昨天其实是不知道怎么做的，因为看到那个example之后就觉得会很复杂，然后也不知道insert和replace的转移方程对应的是什么，还是缺乏耐心。
#### c++ code[28ms 11.3MB]
```
class Solution {
public:
    int minDistance(string word1, string word2) {
        int l1 = word1.size();
        int l2 = word2.size();

        vector<vector <int>> dp(l1+1, vector<int> (l2+1, 0));
        for (int i = 0; i < l1+1; i++) dp[i][0]=i;
        for (int j = 0; j< l2+1;j++) dp[0][j]=j;
        
        for(int i=1;i<dp.size();i++){
            for(int j=1;j<dp[0].size();j++){
                if(word1[i-1]==word2[j-1]) dp[i][j]=dp[i-1][j-1];
                else dp[i][j]=min(min(dp[i-1][j]+1, dp[i][j-1]+1),dp[i-1][j-1]+1);
            }
        }
        return dp[l1][l2];
    }
};
```
二维数组简化成一维
#### c++ code[12ms 8.8MB]
```
class Solution {
public:
    int minDistance(string word1, string word2) {
        int l1 = word1.size();
        int l2 = word2.size();

        vector<int> dp(l2+1, 0);
        vector<int> temp(l2+1, 0);
        for (int j = 0; j< l2+1;j++) temp[j]=j;
        dp.assign(temp.begin(),temp.end());
        
        for(int i=1;i<l1+1;i++){
            for(int j=1;j<l2+1;j++){
                if(word1[i-1]==word2[j-1]) dp[j]=temp[j-1];
                else dp[j]=min(min(temp[j]+1, dp[j-1]+1),temp[j-1]+1);
            }
            dp[0]=i;
            temp.assign(dp.begin(),dp.end());
            // dp[0]=i;
        }
        return dp[l2];
    }
};
```
dp[0]=i那句没有想明白为什么要放在assign前面，但是找了个反例给画出来了知道了是这里写错了。
