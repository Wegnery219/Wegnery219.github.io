---
title: Leetcode300
tags: code
comment: true
date: 2019-09-04 12:09:00
---
### 题目描述
给定一个无序的整数数组，找到其中最长上升子序列的长度。
示例:
输入: [10,9,2,5,3,7,101,18]
输出: 4 
解释: 最长的上升子序列是 [2,3,7,101]，它的长度是 4。
说明:
可能会有多种最长上升子序列的组合，你只需要输出对应的长度即可。
你算法的时间复杂度应该为 O(n2) 。
进阶: 你能将算法的时间复杂度降低到 O(n log n) 吗?

[链接：https://leetcode-cn.com/problems/longest-increasing-subsequence](https://leetcode-cn.com/problems/longest-increasing-subsequence)
### 解
O(n2)的做法为动态规划，`length(j)=max(length(j),1+length(i)) i<j`，O(n log n)的做法为维护一个返回list dp，从头到尾遍历一次list，如果当前value大于dp中的最大值，就添加到最后，否则将dp中第一个大于当前value的值替换为当前value。查找方式使用二分查找。
学习了python的bisect，`bisect.bisect_right(list, target, begin, len(list))` == `bisect.bisect(list, target, begin, len(list))`返回一个index，index含义为第一个大于target的下标，如果存在和target相等的数，则在右边插入，这种情况要小心会越界。等价于c++ algorithm中的`upper_bound(list.begin(),list.end(),target)-list.begin()`，单独的upper_bound返回一个迭代器类型。
为了避免越界可以用`bisect.bisect_left`和`lower_bound`，用法相同，但是如果相等，会返回左边的下标。
#### python code [1016ms 11.9MB] N^2
```
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums == []:
            return 0
        length = len(nums)
        dp = [1] * length
        for i in range(1, length):
            for j in range(i):
                if nums[j] < nums[i]:
                    dp[i] = max(dp[i],1+dp[j])
        return max(dp)
```
#### python code [40ms 11.9MB] N*log(N)
```
class Solution(object):
    def binarysrc(self, dp, begin, end, target):
        while begin<=end:
            mid = (begin+end) // 2
            if target <= dp[mid]:
                end = mid - 1
            else:
                begin = mid + 1
        return begin

    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums == []:
            return 0
        dp = []
        length = len(nums)
        dp.append(nums[0])
        j = 0
        for i in range(1, length):
            if nums[i]>dp[j]:
                dp.append(nums[i])
                j = j + 1
            else:
                index = self.binarysrc(dp, 0, j, nums[i])
                dp[index] = nums[i]
        return len(dp)
```
#### python code [40ms 11.9MB] N*log(N) use bisect
```
import bisect
class Solution(object):
    def lengthOfLIS(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        if nums == []:
            return 0
        length = len(nums)
        dp = []
        dp.append(nums[0])
        j = 0
        for i in range(1, length):
            if nums[i]>dp[j]:
                dp.append(nums[i])
                j = j + 1
            else:
                index = bisect.bisect_left(dp, nums[i], 0, j+1)
                dp[index] = nums[i]
        return len(dp)
```
#### C++ code [8ms 8.5MB]
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
