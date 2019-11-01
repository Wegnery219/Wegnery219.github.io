---
title: Leetcode 300. Longest Increasing Subsequence
tags: code
comment: true
date: 2019-11-1 12:09:00
---
### 题目描述
Given an unsorted array of integers, find the length of longest increasing subsequence.
Example:
Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
Note:
There may be more than one LIS combination, it is only necessary for you to return the length.
Your algorithm should run in O(n2) complexity.
Follow up: Could you improve it to O(n log n) time complexity?
### 解
做二分的时候做过这道题，但是看到算法导论上15.4章最后的思考题是这个，然后用了和最长公共子序列差不多的做法，复杂度为O(n^2),当时是直接用二分做的，复杂度是O(nlogn)。
1. O(n^2)
复制一个数组s'，对s’进行排序,找s和s'的最长公共子序列。这种做法的好处就是可以存下来最后的LIS是什么，当然对这道题不是很必要。而且用了很多额外的存储。缺点就是解决不了重复，如果原始数组中存在重复数组，需要对s‘进行去重。
#### c++ code[140ms 77MB]
```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> numsnew=nums;
        sort(nums.begin(),nums.end());
        nums.erase(unique(nums.begin(),nums.end()),nums.end());
        vector<vector<int>> record(numsnew.size()+1,vector<int> (nums.size()+1, 0));
        int length=0;

        for(int i=1;i<=numsnew.size();i++){
            for (int j = 1; j <= nums.size(); j++)
            {
                if (nums[j-1]==numsnew[i-1])
                {
                    record[i][j]=record[i-1][j-1]+1;
                }
                else if(record[i-1][j]>=record[i][j-1]) record[i][j]=record[i-1][j];
                else record[i][j]=record[i][j-1];
            }
            
        }
        
        return record[numsnew.size()][nums.size()];
    }
};
```
这里其实可以不用二维的数组的。但是不太会怎么改。
2. O(nlogn)
利用的思想是一个长度为i的候选子序列的尾元素至少不比一个长度为i-1的候选子序列的尾元素小。书上这么写的，就是维护一个数组。但是缺点是这个数组里存的并不是最终的LIS。比如[1,3,4,6,2]最后数组里是[1,2,4,6]，并不是最终的结果，具体过程参考[这里](https://www.felix021.com/blog/read.php?1587)
#### c++ code
```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        if(nums.size()==0){
            return 0;
        }
        int length = nums.size();
        vector<int> dp;
        dp.push_back(nums[0]);
        int j = 0;
        for(int i=1; i<length;i++){
            if(nums[i]>dp[j]){
                dp.push_back(nums[i]);
                j = j + 1;
            }
            else{
                int index = lower_bound(dp.begin(), dp.end(), nums[i]) - dp.begin();
                dp[index] = nums[i];

            }
        }
        return dp.size();
    }
};
```
