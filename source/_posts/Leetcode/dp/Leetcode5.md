---
title: Leetcode5.Longest Palindromic Substring
date: 2019-12-03 14:51:40
tags: code
---
### [题目描述](https://leetcode.com/problems/longest-palindromic-substring/)
Given a string s, find the longest palindromic substring in s. You may assume that the maximum length of s is 1000.

Example 1:

Input: "babad"
Output: "bab"
Note: "aba" is also a valid answer.
Example 2:

Input: "cbbd"
Output: "bb"
### 解
1. 利用lcs(string不是sequence,string和sequence算法的区别是如果char不相等，dp数组不用取最大值，直接=0)来做，将原始字符串reverse之后和原始字符串找最长公共子字符串。这样做会有几个用例通过不了，如'aacdefcaa'的true answer为aa，而用该算法会返回aac，因为原始字符串中出现了倒序('aac'和'caa')导致将该字符串反转之后为'aacfedcaa'和原始字符串'aacdefcaa'的lcs为aac,而aac本身并不是一个palindromic。解决方法是每次找到相等的字符的时候根据当前回文长度length和下标index推出来的originj和当前原始串中的i是否相等来确定是不是同一个串得出来的。比如说‘abbae’，倒过来是‘eabba’，在考虑'abba'最后这个'a'的时候，先找到在原始'abbae'中的对应的位置，应该对应的是第一个，然后加上它目前的长度4，和当前i正好相等，而'aacfedcaa'中的c反推的时候反推到了最后一个a上，不是对应的c。
#### c++ code[308ms 186.9MB]
```
class Solution {
public:
    struct Result
    {
        int i;
        int biggest;
    };
    
    string longestPalindrome(string s) {
        string palind="";
        string s2=s;
        reverse(s.begin(),s.end());
        vector<vector<int>> dp(s.size()+1, vector<int> (s.size()+1,0));

        struct Result res = LCS(s2,s,dp);
        if(res.biggest==0) return "";
        while (res.biggest!=0)
        {
            palind+=s2[res.i-1];
            res.i--;
            res.biggest--;
        }
        return palind;
    }

    struct Result LCS(const string &s1, const string &s2, vector<vector<int>> &dp){
        struct Result res = {0,0};
        int biggest=0;
        // edge judge
        if(s1==""||s2=="") return res;

        // dp begin
        for (int i = 1; i < s1.size()+1; i++)
        {
            for (int j = 1; j < s2.size()+1; j++)
            {
                if (s1[i-1]==s2[j-1])
                {
                    dp[i][j]=dp[i-1][j-1]+1;
                }
                if(dp[i][j]>biggest){
                    int prej=s2.size()-j;
                    int nowj=prej+dp[i][j];
                    if(i==nowj){
                       biggest=dp[i][j];res.i=i;res.biggest=biggest; 
                    }
                }
            }
        }
        return res;
    }
};
```
2. 枚举center的位置，如果s[left]==s[right]则left--,right++,注意的是不是枚举n个位置，而是2n-1个位置，因为如果palindrome的长度是偶数的话，center是中间那个档，如'bb'
#### c++ code[16ms 8.7MB]
```
class Solution {
public:
    string longestPalindrome(string s) {
        if (s.size()==0) return "";
        int biggest=0;
        int begin=0;
        int end=0;
        for(int i=0;i<s.size();i++){
            int len1=expandcenter(s,i,i);
            int len2=expandcenter(s,i,i+1);
            int tempbiggest=max(len1,len2);
            if(tempbiggest>biggest){
                biggest=tempbiggest;
                end = i + tempbiggest/2;
                begin = end - tempbiggest + 1;
            }
        }
        return s.substr(begin, biggest);
    }
    // return length of palind
    int expandcenter(const string &s, int left, int right){
        while (left>=0 && right<s.size() && s[left]==s[right])
        {
            left--;right++;
        }
        return right-left-1;
    }
};
```
注意最后这里是right-left-1因为此时right和left是已经++和--过的，是不符合s[right]==s[left]的right-left-1=(right-1)-(left+1)+1,一开始写错了直接写成right-left+1了。
