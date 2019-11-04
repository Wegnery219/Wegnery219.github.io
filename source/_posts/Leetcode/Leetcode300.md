---
title: Leetcode300:Longest Increasing Subsequence
tags: code
comment: true
date: 2019-11-03 12:09:00
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

O(n^2)的另一种做法，来自于算法导论第十五章的课后习题

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
这里其实可以不用二维的数组的。改成用两个一维数组。
#### c++ code[116ms 8.9MB]
```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        vector<int> numsnew=nums;
        sort(nums.begin(),nums.end());
        nums.erase(unique(nums.begin(),nums.end()),nums.end());
        vector<int> dp(nums.size()+1, 0);
        vector<int> temp(nums.size()+1,0);

        for(int i=1;i<=numsnew.size();i++){
            for (int j = 1; j <= nums.size(); j++)
            {
                if (nums[j-1]==numsnew[i-1])
                {
                    dp[j]=temp[j-1]+1;
                }
                else if(temp[j]>=dp[j-1]) dp[j]=temp[j];
                else dp[j]=dp[j-1];
            }
            temp.assign(dp.begin(),dp.end());
        }
        
        return dp[nums.size()];
    }
};
```
内存相比二维数组还是省了很多的，但是跟二分做法相比还是蠢。
O(nlogn)
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
