---
title: 32. Longest Valid Parentheses
date: 2019-12-06 21:47:23
tags: code
---
### [题目描述](https://leetcode.com/problems/longest-valid-parentheses/)
Given a string containing just the characters '(' and ')', find the length of the longest valid (well-formed) parentheses substring.

Example 1:

Input: "(()"
Output: 2
Explanation: The longest valid parentheses substring is "()"
Example 2:

Input: ")()())"
Output: 4
Explanation: The longest valid parentheses substring is "()()"

### 解
1. 动态规划
状态:当前str含有几个char
选择:也不能算选择吧，就是根据当前s[i]是什么判断从哪个状态转移
状态转移方程:
dp[i]=dp[i-2]+2 if s[i]==')' and s[i-1]=='(' 这里之前写成dp[i-1]+2了，会出现'(()()'算完就等于2而不是4的wrong case，因为遇到'('时会直接清0
dp[i]=dp[i-dp[i-1]-2]+dp[i-1]+2 if s[i]==')' and s[i-1]==')' and s[i-dp[i-1]-1] == '(',就是出现'((())'这种套起来的情况的处理case
dp[i]=0 else
#### python code[52ms 12.7MB]
```
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        length=len(s)
        maxlength=0
        dp=[0 for _ in range(length)]
        for i in range(length):
            if i==0:
                dp[i]=0
            elif s[i]==')' and s[i-1]=='(':
                dp[i]=dp[i-2]+2
            elif s[i]==')' and s[i-1]==')':
                if (i-dp[i-1]-1)>=0 and s[i-dp[i-1]-1] == '(':
                    if (i-dp[i-1]-2)>=0:
                        dp[i]=dp[i-dp[i-1]-2]+dp[i-1]+2
                    else:
                        dp[i]=dp[i-1]+2
            else:
                dp[i]=0
            if dp[i]>maxlength:
                maxlength=dp[i]
        return maxlength
```
2. 栈
将坐标push进栈，具体解释详见：[solution](https://leetcode.com/problems/longest-valid-parentheses/solution/)
出现了一次RuntimeError,因为没有考虑栈为空的情况，括号问题遇到栈的解决方法是遇到'('就push进栈，遇到')'就pop，所以要在栈里多留一个不属于str index的元素，避免空栈pop
#### c++ code[0ms 9.6MB]
```
class Solution {
public:
    int longestValidParentheses(string s) {
        stack<int> st;
        int maxlength=0;
        st.push(-1);
        for (int i = 0; i < s.size();i++)
        {
            if(s[i]=='(') st.push(i);
            else{
                st.pop();
                if(st.size()==0) st.push(i);
                int currlength=i-st.top();
                if(maxlength<currlength) maxlength=currlength;
            }
        }
        return maxlength;
    }
};
```
3. 左右遍历的方法
详解也见[solution](https://leetcode.com/problems/longest-valid-parentheses/solution/)
这个虽然看懂了，但是还是不太明白原理，让我再想还是想不出来，不过从左到右，从右到左这个在之前转圈抢银行不能抢相邻的这个题里用过。
#### c++ code[8ms 9MB]
```
class Solution {
public:
    int longestValidParentheses(string s) {
        int length=s.size();
        int left=0;
        int right=0;
        int maxlength=0;
        for (int i = 0; i < length; i++)
        {
            if(s[i]=='(') left++;
            else right++;
            if (left==right) maxlength=max(left+right, maxlength);
            else if(right>left) left=0,right=0;
        }
        left=0;
        right=0;
        for (int i = length-1; i >=0 ; i--)
        {
            if(s[i]=='(') left++;
            else right++;
            
            if (left==right) maxlength=max(left+right, maxlength);
            else if(left>right) left=0,right=0;
        }
        return maxlength;
    }
};
```
