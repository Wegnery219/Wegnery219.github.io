---
title: 10. Regular Expression Matching
date: 2019-11-26 12:40:14
tags: code
---
### [题目描述](https://leetcode.com/problems/regular-expression-matching/)
Given an input string (s) and a pattern (p), implement regular expression matching with support for '.' and '*'.

'.' Matches any single character.
'*' Matches zero or more of the preceding element.
The matching should cover the entire input string (not partial).

Note:

s could be empty and contains only lowercase letters a-z.
p could be empty and contains only lowercase letters a-z, and characters like . or *.
Example 1:

Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".
Example 2:

Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".
Example 3:

Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".
Example 4:

Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab".
Example 5:

Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
### 解
该问题可以分解成子问题，s[:2],p[:2]是否匹配可以由s[:1],p[:1]转移而来
状态：s[:i] p[:j]里面的i,j，当前匹配的是哪两个字符串
选择：如果s[i]==p[j]，则s[:i] p[:j]是否匹配可以由s[:i-1] p[:j-1]是否匹配得到。(解释：s[:2]和p[:2]匹配等价于s[:1]和p[:1]匹配并且s[2]==p[2])
如果当前p中为`*`，则可以选择忽略`*`号前的字母或者如果s[i]==p[j-1]的话，匹配`*`号前的字母
动态规划转移方程：
```
if p[j]==s[i] dp[i][j]=dp[i-1][j-1]
else dp[i][j]=false

if p[j]=='*':
    dp[i][j]=dp[i][j-2] || (s[i]==p[j-1] || p[j-1]=='.') && dp[i-1][j]
```
初始化：
dp[0][0]=true 两个啥都没有的字符串可以匹配
如果s不为空，p为空，dp[i][0]=false
如果s为空，p不为空，dp[0][j]可能是true，有可能出现p=`c*d*e*`的情况，全都重复0次。
#### c++ code[0ms 8.2MB]
```
class Solution {
public:
    bool charMatch(char a, char b){
        if (a==b || b=='.')
        {
            return true;
        }
        return false;
    }

    bool isMatch(string s, string p) {
        int m=s.size();
        int n=p.size();
        bool dp[m+1][n+1];

        dp[0][0]=true;
        for(int i=1;i<=m;i++) dp[i][0]=false;
        for(int j=1;j<=n;j++) dp[0][j]=(j>1) && (p[j-1]=='*') && dp[0][j-2]==true;

        for(int i=1;i<=m;i++){
            for (int j=1;j<=n;j++)
            {
                if(p[j-1]=='*'){
                    dp[i][j]=dp[i][j-2];
                    if (charMatch(s[i-1],p[j-2]))
                    {
                        dp[i][j] |= dp[i-1][j];
                    }
                }
                else{
                    if(charMatch(s[i-1],p[j-1])) dp[i][j]=dp[i-1][j-1];
                    else dp[i][j]=false;
                }
            }
        }

        return dp[m][n];
    }
};
```
#### python code[56ms 12.8MB]
```
class Solution:
    def charMatch(self, a: str, b:str) -> bool:
        if a==b or b=='.':
            return True
        else:
            return False

    def isMatch(self, s: str, p: str) -> bool:
        n = len(s)
        m = len(p)

        dp = [[[] for _ in range(m+1)] for _ in range(n+1)]
        dp[0][0]=True

        for i in range(1,n+1):
            dp[i][0]=False
        for j in range(1,m+1):
            dp[0][j]=(j>1) and p[j-1]=='*' and dp[0][j-2]
        
        for i in range(1, n+1):
            for j in range(1, m+1):
                if p[j-1]=='*':
                    dp[i][j] = dp[i][j-2]
                    if self.charMatch(s[i-1],p[j-2]):
                        dp[i][j] = dp[i-1][j] or dp[i][j]
                else:
                    if self.charMatch(s[i-1],p[j-1]):
                        dp[i][j] = dp[i-1][j-1]
                    else:
                        dp[i][j] = False

        return dp[-1][-1]
```
