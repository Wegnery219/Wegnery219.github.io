---
title: Leetcode53:Maximum Subarray
tags: code
comment: true
date: 2019-10-22 12:09:00
---
### 题目描述
Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
Example:
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
Follow up:
If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.
### 解
动态规划:
DP[i]=DP[i-1]+nums[i] if DP[i-1]>=0
DP[i]=nums[i] if DP[i-1]<0
#### python code[68ms 14.4MB]
```
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        record=[]
        record.append(nums[0])
        for i in range(1, len(nums)):
            if record[i-1]<0:
                record.append(nums[i])
            else:
                record.append(record[i-1]+nums[i])
        return max(record)
```
#### C++ code[8ms 9.6MB]
```
#include<algorithm>
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int> record;
        record.push_back(nums[0]);
        for(int i=1;i<nums.size();i++){
            if(record[i-1]<0) record.push_back(nums[i]);
            else{
                record.push_back(record[i-1]+nums[i]);
            }
        }
        //int position=0;
        int position=max_element(record.begin(),record.end())-record.begin();
        return record[position];
    }
};
```
计算一个vector中的最大值可以用max_element函数
WA1:`runtime error: reference binding to null pointer of type 'value_type' (stl_vector.h)`赋值vector的时候没有用pushback，直接vector[0]=...，所以是空指针错。
最后不用`int position;`，可以直接*取出元素`return *max_element(record.begin(),record.end());`[4ms 9.6MB]
不用vector的方法[8ms 9.1MB]
```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int sum=nums[0];
        int maxmium=nums[0];
        for(int i=1;i<nums.size();i++){
            if(sum<0) sum=nums[i];
            else sum+=nums[i];
            if(sum>maxmium) maxmium=sum;
        }
        return maxmium;
    }
};
```
