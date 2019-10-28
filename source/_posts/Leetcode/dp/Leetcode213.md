---
title: Leetcode 213. House Robber II
tags: code
comment: true
date: 2019-10-28 12:09:00
---
### 题目描述
You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed. All houses at this place are arranged in a circle. That means the first house is the neighbor of the last one. Meanwhile, adjacent houses have security system connected and it will automatically contact the police if two adjacent houses were broken into on the same night.
Given a list of non-negative integers representing the amount of money of each house, determine the maximum amount of money you can rob tonight without alerting the police.
Example 1:
Input: [2,3,2]
Output: 3
Explanation: You cannot rob house 1 (money = 2) and then rob house 3 (money = 2),because they are adjacent houses.
Example 2:
Input: [1,2,3,1]
Output: 4
Explanation: Rob house 1 (money = 1) and then rob house 3 (money = 3).Total amount you can rob = 1 + 3 = 4.
### 解
啊哈巧的是我矩阵课之前刚好看了这个[泛化的动态规划](https://sf-zhou.github.io/algorithm/generic_dp.html?nsukey=OEbNNZuZPXVcSfrnbzHZ9x%2FQtOOEjs9DJNXyGuDFAaKpRFgt1K%2BKg8ZTJUHPc77AlEcBwMqEH92OHpD3uPgrWQtZNDoF9g0nLQd214qY6seOZBQfXG%2BKqHwbxph9Pp8O7PIK8fpqAqCp9g1ADka84Qe%2F60EUqojDiMrJB26n1a2vGwXHn%2FyYdNhQumHxJ09Bh%2FgHSs%2FlRR80CDQP4Mz3HQ%3D%3D)，然后这个题我就会了。有个条件就是头和尾是相连的，做的时候要想办法避开。
#### python code[52ms 13.8MB]
```
class Solution:
    def helper(self, nums: List[int]) -> int:
        size = len(nums)
        if size==0:
            return 0
        record = [[0 for _ in range(2)] for _ in range(size)]
        record[0][1] = nums[0]
        for i in range(1, size):
            record[i][1] = nums[i]+record[i-1][0]
            record[i][0] = max(record[i-1][0], record[i-1][1])
        return max(record[-1][1], record[-1][0])
    def rob(self, nums: List[int]) -> int:
        size = len(nums)
        if size==0:
            return 0
        if size==1:
            return nums[0]
        return max(self.helper(nums[1:]), self.helper(nums[:-1]))
```
WA1:没有考虑size=1
#### python code[8ms 8.9MB]
```
class Solution {
public:
    int helper(vector<int>& nums, int left, int right){
        int size = right-left+1;
        if(size==0) return 0;
        vector<int> vec2(2,0);
        vector<vector<int>> record(size, vec2);
        record[0][1]=nums[left];
        for(int i=1;i<size;i++){
            record[i][1] = nums[i+left]+record[i-1][0];
            record[i][0] = max(record[i-1][0], record[i-1][1]);
        }
        return max(record[size-1][1], record[size-1][0]);
    }
    int rob(vector<int>& nums) {
        int size=nums.size();
        if(size==0) return 0;
        if(size==1) return nums[0];
        return max(helper(nums, 1, size-1), helper(nums, 0, size-2));
    }
};
```
