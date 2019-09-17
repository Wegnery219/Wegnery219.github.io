---
title: Leetcode153+154
tags: code
comment: true
date: 2019-09-17 12:09:00
---
## Leetcode153
### 题目描述
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).
Find the minimum element.
You may assume no duplicate exists in the array.
Example 1:
Input: [3,4,5,1,2] 
Output: 1
Example 2:
Input: [4,5,6,7,0,1,2]
Output: 0

[链接：https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array)
### 解
二分查找，返回条件为nums[mid]<nums[mid-1]，要判断mid-1是否越界，特殊情况需要考虑的为[1,2,3]无旋转的上升数组，这种情况通过二分是找不到min的，会跳出while循环，因为是上升数组，所以跳出循环直接返回nums[0]。
#### python code[44ms 12.1MB]
```
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        left = 0
        right = len(nums)
        if right == 0:
            return -1
        if right==1:
            return nums[right-1]
        while left<right:
            mid = (left+right)//2
            if mid-1<0:
                return nums[mid]
            if nums[mid]<nums[mid-1]:
                return nums[mid]
            elif nums[mid]>=nums[0]:
                left = mid + 1
            else:
                right = mid
        return nums[0]
```
#### c++ code[4ms 8.6MB]
```
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0;
        int right = nums.size();
        if(right == 0) return -1;
        if(right == 1) return nums[0];
        while (left<right)
        {
            int mid = (left+right)/2;
            if (mid-1<0) return nums[mid];
            if (nums[mid]<nums[mid-1]) return nums[mid];
            else if(nums[mid]>=nums[0]) left = left + 1;
            else right = mid;
        }
        return nums[0];
    }
};
```
## Leetcode154
### 题目描述
Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
(i.e.,  [0,1,2,4,5,6,7] might become  [4,5,6,7,0,1,2]).
Find the minimum element.
The array may contain duplicates.
Example 1:
Input: [1,3,5]
Output: 1
Example 2:
Input: [2,2,2,0,1]
Output: 0
Note:
This is a follow up problem to Find Minimum in Rotated Sorted Array.
Would allow duplicates affect the run-time complexity? How and why?

[链接：https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/find-minimum-in-rotated-sorted-array-ii)
### 解
和上一题的区别是存在重复元素，把头尾都存在的重复元素掐掉，保留一边就可以沿用上一题的方法了，复杂度为O(N)
#### python code[48ms 12MB]
```
class Solution(object):
    def findMin(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        left = 0
        right = len(nums)
        if right == 0:
            return -1
        if right == 1:
            return nums[0]
        if nums[0]==nums[-1]:
            mark = nums[-1]
            while right-1>=0 and nums[right-1]==mark:
                right = right - 1
            if right==0:
                return mark
        while left<right:
            mid = (left+right)//2
            if mid-1<0:
                return nums[mid]
            if nums[mid]<nums[mid-1]:
                return nums[mid]
            elif nums[mid]>=nums[0]:
                left = left + 1
            else:
                right = mid
        return nums[0]
```
#### c++ code[12ms 8.8MB]
```
class Solution {
public:
    int findMin(vector<int>& nums) {
        int left = 0;
        int right = nums.size();
        if(right==0) return -1;
        if(right==1) return nums[0];
        if(nums[0]==nums[right-1]){
            int mark = nums[0];
            while(right-1>=0 && nums[right-1]==mark) right = right - 1;
            if(right==0) return mark;
        }
        while(left<right){
            int mid = (left+right)/2;
            if(mid-1<0) return nums[mid];
            if(nums[mid]<nums[mid-1]) return nums[mid];
            if(nums[mid]>=nums[0]) left = left + 1;
            else right = mid;
        }
        return nums[0];
    }
};
```
