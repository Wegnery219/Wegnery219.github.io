---
title: Leetcode647.Palindromic Substrings
date: 2019-12-04 09:34:38
tags: code
---
### [题目描述](https://leetcode.com/problems/palindromic-substrings/)
Given a string, your task is to count how many palindromic substrings in this string.

The substrings with different start indexes or end indexes are counted as different substrings even they consist of same characters.

Example 1:

Input: "abc"
Output: 3
Explanation: Three palindromic strings: "a", "b", "c".
 

Example 2:

Input: "aaa"
Output: 6
Explanation: Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa".
 

Note:

The input string length won't exceed 1000.
### 解
和昨天的longgest palindrome相同，这道题的想法也是从中间开始往外扩张，不一样的地方是只要s[left]==s[right]count就+1，因为说明此时存在palindrome了
#### c++ code[4ms 8.5MB]
```
class Solution {
public:
    int countSubstrings(string s) {
        if(s=="") return 0;
        int count=0;
        for (int i = 0; i < s.size(); i++)
        {
            count += fromcenter(s, i, i);
            count += fromcenter(s, i, i+1);
        }
        return count;
    }

    int fromcenter(const string &s, int left, int right){
        int count=0;
        while (left>=0 && right<s.size() && s[left]==s[right])
        {
            count++;
            left--;
            right++;
        }
        return count;
    }
};
```
