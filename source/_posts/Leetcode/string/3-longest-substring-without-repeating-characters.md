---
title: 3.longest-substring-without-repeating-characters
date: 2019-12-09 15:19:20
tags: code
---
### [题目描述](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
Given a string, find the length of the longest substring without repeating characters.

Example 1:

Input: "abcabcbb"
Output: 3 
Explanation: The answer is "abc", with the length of 3. 
Example 2:

Input: "bbbbb"
Output: 1
Explanation: The answer is "b", with the length of 1.
Example 3:

Input: "pwwkew"
Output: 3
Explanation: The answer is "wke", with the length of 3. 
             Note that the answer must be a substring, "pwke" is a subsequence and not a substring.
### 解
想法是利用dp,如果s[i]在前面以s[i-1]结尾的substr中没有出现，则dp[i]=dp[i-1]+1,否则dp[i]=当前下标i-（出现的那个位置+1）+1,就相当于换了个头，从重复那个地方开始。这里不用考虑什么万一有多个重复该找哪一个的问题，因为万一多个重复，第一次遇到重复的时候就已经把重复的char排除出去了。
#### c++ code[8ms 9.9MB]
```
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        vector<int> dp(s.size()+1,0);
        int maxlength=0;
        for(int i=1;i<s.size()+1;i++){
            int index = s.find(s[i-1], i-1-dp[i-1]);
            if(index!=s.npos && index<i-1){
                dp[i]=i-index-1;
            }
            else
                dp[i]=dp[i-1]+1;
        }
        return *max_element(dp.begin(), dp.end());
    }
};
```
#### python code[44ms 13.1MB]
```
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        dp = [0 for _ in range(len(s)+1)]
        for i in range(1, len(s)+1):
            index=s.find(s[i-1], i-1-dp[i-1], i-1)
            if index != -1:
                # dp[i]= i-1-(index+1)+1
                dp[i] = i-index-1
            else:
                dp[i]=dp[i-1]+1
        return max(dp)
```
要记住c++里的string.find和python里str.find的用法，c++里面如果没找到返回string.npos,找到了返回下标，输入参数只有起始position,python里是起始position,end position,当然end position不包括在里面，找到了返回index，没找到返回-1
看了题解，题解里的想法跟我的差不多，但是没有用find方法，用了一个hash set来存当前的substr，等复习一下hash然后重写一下这个题吧。先记住str.find的用法。
