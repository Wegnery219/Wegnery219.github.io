---
title: 20. Valid Parentheses
date: 2019-12-07 12:56:50
tags: code
---
### [题目描述](https://leetcode.com/problems/valid-parentheses/)
Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

Open brackets must be closed by the same type of brackets.
Open brackets must be closed in the correct order.
Note that an empty string is also considered valid.

Example 1:

Input: "()"
Output: true
Example 2:

Input: "()[]{}"
Output: true
Example 3:

Input: "(]"
Output: false
Example 4:

Input: "([)]"
Output: false
Example 5:

Input: "{[]}"
Output: true
### 解
用栈
#### c++ code[4ms 8.4MB]
```
class Solution {
public:
    bool isValidchar(char c, char top){
        if(c==')' && top=='(') return true;
        if(c=='}' && top=='{') return true;
        if(c==']' && top=='[') return true;
        return false;
    }
    bool isValid(string s) {
        stack<char> st;
        for(int i=0;i<s.size();i++){
            if (s[i]=='(' or s[i]=='[' or s[i]=='{') st.push(s[i]);
            else{
                if(st.empty()) return false;
                else if (isValidchar(s[i], st.top()))
                {
                    st.pop();
                }
                else return false;
            }
        }
        if(st.empty()) return true;
        else return false;
    }
};
```
