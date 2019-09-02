---
title: Leetcode34
tags: code
comment: true
---
### 题目描述：
```
在排序数组中查找元素的第一个和最后一个位置
给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

你的算法时间复杂度必须是 O(log n) 级别。

如果数组中不存在目标值，返回 [-1, -1]。

示例 1:

输入: nums = [5,7,7,8,8,10], target = 8
输出: [3,4]
示例 2:

输入: nums = [5,7,7,8,8,10], target = 6
输出: [-1,-1]

链接：https://leetcode-cn.com/problems/find-first-and-last-position-of-element-in-sorted-array
```

### 解
使用二分查找

#### python code[160ms 13.1MB]
```
class Solution(object):
    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        begin=-1
        end = -1
        length = len(nums)
        left = 0
        right = length-1

        while left<=right:
            mid = (left+right)/2
            if nums[mid] == target:
                m = mid
                begin = mid
                end = mid
                while begin>0 and nums[begin]==nums[begin-1]:
                    begin = begin - 1
                while end<length-1 and nums[end]==nums[end+1]:
                    end = end + 1
                return[begin, end]
            elif nums[mid] > target:
                right = mid - 1
            else:
                left = mid + 1
        return [begin,end]
```
执行时间慢的原因是中间找target的begin和end的时候时间复杂度太高，如果一个数组中只有一个元素，时间复杂度为O(N),所以在找begin和end的时候，也用二分查找。

#### python code[100ms 13.1MB]
```
class Solution(object):
    def left_firstnot(self, nums, begin, end, target):
        while begin<=end:
            mid = (begin+end)//2
            if nums[mid] == target:
                end = mid - 1
            elif nums[mid]<target:
                begin = mid + 1
        return begin
    
    def right_firstnot(self, nums, begin, end, target):
        while begin<=end:
            mid = (begin+end) // 2
            if nums[mid] == target:
                begin = mid + 1
            elif nums[mid] > target:
                end = mid - 1
        return end

    def searchRange(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        begin=-1
        end = -1
        length = len(nums)
        left = 0
        right = length-1

        while left<=right:
            mid = (left+right)//2
            if nums[mid] == target:
                begin = self.left_firstnot(nums, left, mid, target)
                end = self.right_firstnot(nums, mid, right, target)
                return[begin, end]
            elif nums[mid] > target:
                right = mid - 1
            else:
                left = mid + 1
        return [begin,end]
```

#### c++ code[8ms 10.3MB]
```
#include<vector>
using namespace std;
class Solution {
public:
    int left_search(vector<int>& nums, int begin, int end , int target){
        while (begin <= end)
        {
            int mid = (begin+end)/2;
            if(nums[mid] == target){
                end = mid - 1;
            }
            else if(nums[mid]<target){
                begin = mid + 1;
            }
        }
        return begin;
    }
    int right_search(vector<int>& nums, int begin, int end, int target){
        while (begin <= end)
        {
            int mid = (begin+end)/2;
            if(nums[mid]==target){
                begin = mid + 1;
            }
            else if(nums[mid]>target){
                end = mid - 1;
            }
        }
        return end;
        
    }
    vector<int> searchRange(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size()-1;
        int begin = -1;
        int end = -1;
        vector<int> res;
        while(left<=right){
            int mid = (left+right)/2;
            if(nums[mid]==target){
                begin = left_search(nums, left, mid, target);
                end = right_search(nums, mid, right, target);
                res={begin,end};
                return res;
            }
            else if (nums[mid]>target)
            {
                right = mid - 1;
            }
            else{
                left = mid + 1;
            }
        }
        res = {begin,end};
        return res;
    }
};
```
啧，c++真快
