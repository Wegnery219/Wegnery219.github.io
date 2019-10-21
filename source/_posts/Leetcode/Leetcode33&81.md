---
title: Leetcode33+81:Search in Rotated Array I&II
tags: code
comment: true
date: 2019-09-16 12:09:00
---
## Leetcode33
### 题目描述
假设按照升序排序的数组在预先未知的某个点上进行了旋转。
( 例如，数组 [0,1,2,4,5,6,7] 可能变为 [4,5,6,7,0,1,2] )。
搜索一个给定的目标值，如果数组中存在这个目标值，则返回它的索引，否则返回 -1 。
你可以假设数组中不存在重复的元素。
你的算法时间复杂度必须是 O(log n) 级别。
示例 1:
输入: nums = [4,5,6,7,0,1,2], target = 0
输出: 4
示例 2:
输入: nums = [4,5,6,7,0,1,2], target = 3
输出: -1

[链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array](https://leetcode-cn.com/problems/search-in-rotated-sorted-array)
### 解
1. 要求了时间复杂度logn,想法是二分，二分有两种情况，一种是target最终位置在左半边，即`target>=nums[0]`，一种是target最终在右半边，在左半边的时候分析过程如下,rotate指数组中的旋转元素，也是数组中的最大元素：
```
1. left--target--rotate---mid---right
此时right=mid
2. left--target--mid--rotate--right 
此时right=mid
3. left--mid--target--rotate--right
此时left=mid+1
```
在右半边的时候：
```
1. left--mid--rotate--target---right
此时left=mid+1
2. left---rotate--mid--target--right
此时left=mid+1
3. left---rotate---target---mid---right
此时right=mid
```
#### c++ code [8ms 8.8MB]
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size()-1;
        if (nums.size()==0)
            return -1;
        while (left<right)
        {
            int mid = (left+right)/2;
            if (target >= nums[0]) //left
            {
                if(nums[mid]<target && nums[mid]>=nums[0])
                    left = mid + 1;
                else
                    right = mid;
            }
            else{
                if(nums[mid]>=target && nums[mid]<nums[0])
                    right = mid;
                else
                    left = mid + 1;
            }
        }
        if (nums[left]==target)
        {
            return left;
        }
        else
            return -1;
    }
};
```
2. 第二种是先二分找出来旋转元素的位置p，然后判断target的位置，在(left,p)之间或者(p,right)之间正常二分。
#### python code[52ms 12MB]
```
class Solution(object):
    def ord_search(self, nums, target,left,right):
        while left<=right:
            mid = (left+right)/2
            if nums[mid]==target:
                return mid
            elif nums[mid]<target:
                left = mid + 1
            else:
                right = mid - 1
        return -1

    def get_rotate(self, nums, left, right):
        length = len(nums)
        if length == 1 or length == 2:
            return 0
        while left<=right:
            mid = (left+right)/2
            if (mid+1)>=len(nums):
                return mid
            if nums[mid]>nums[mid+1]:
                return mid
            elif nums[mid]>=nums[left]:
                left = mid + 1
            else:
                right = mid - 1
        return -1

    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if len(nums)==0:
            return -1
        rotate = self.get_rotate(nums, 0, len(nums)-1)
        if target>=nums[0] and target<=nums[rotate]:
            return self.ord_search(nums, target, 0, rotate)
        else:
            return self.ord_search(nums, target, rotate+1, len(nums)-1)
```
代码写的好像有点恶心咳咳
## Leetcode81
### 题目描述
假设按照升序排序的数组在预先未知的某个点上进行了旋转。
(例如，数组 [0,0,1,2,2,5,6] 可能变为 [2,5,6,0,0,1,2])。
编写一个函数来判断给定的目标值是否存在于数组中。若存在返回 true，否则返回 false。
示例 1:
输入: nums = [2,5,6,0,0,1,2], target = 0
输出: true
示例 2:
输入: nums = [2,5,6,0,0,1,2], target = 3
输出: false
进阶:
这是 搜索旋转排序数组 的延伸题目，本题中的 nums  可能包含重复元素。
这会影响到程序的时间复杂度吗？会有怎样的影响，为什么？

[链接：https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii](https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii)
### 解
和上一个题一样，但是多了重复的元素，思路是把重复元素掐头去尾，只在最前面保留一个重复元素，这样就可以同样用leetcode33的算法啦
#### python code[96ms 11.9MB]
```
class Solution(object):
    def ord_search(self, nums, target,left,right):
        while left<=right:
            mid = (left+right)/2
            print(left)
            print(right)
            if nums[mid]==target:
                return True
            elif nums[mid]<target:
                left = mid + 1
            else:
                right = mid - 1
        return False
    def get_rotate(self, nums, left, right):
        length = right-left+1
        if length == 1:
            return left
        if length == 2:
            if nums[right]>nums[left]:
                return right
            else:
                return left
        while left<=right:
            mid = (left+right)/2
            if (mid+1)>=len(nums):
                return mid
            if nums[mid]>nums[mid+1]:
                return mid
            elif nums[mid]>=nums[left]:
                left = mid + 1
            else:
                right = mid - 1
        return -1
    # def get_rid(self,nums):
    #     left = 0
    #     right = len(nums)-1
    #     mid = (left+right)/2
    #     res = []
    #     if nums[mid]>nums[left]:
    #         left = bisect.bisect_right(nums,nums[0],0,mid+1)
    #         res.append(left)
    #     elif nums[mid]<nums[left]:
    #         right = bisect.bisect_left(nums, nums[0],mid,right-mid+1)

    #     else: #nums[mid]==nums[left]
    def get_rid(self, nums):
        mark = nums[0]
        left = 0
        right = len(nums)-1
        while left < len(nums) and nums[left]==mark:
            left = left + 1
        while right > -1 and nums[right]==mark:
            right = right - 1
        return left,right
    def search(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        if len(nums)==0:
            return False
        left = 0
        right = len(nums)-1
        if nums[0]==nums[-1]:
            if nums[0]==target:
                return True
            else:
                left, right = self.get_rid(nums)
                left = left - 1
                if right == -1:
                    return False
        print(left)
        print(right)
        rotate = self.get_rotate(nums, left, right)
        print(rotate)
        if target>=nums[left] and target<=nums[rotate]:
            return self.ord_search(nums, target, 0, rotate)
        else:
            return self.ord_search(nums, target, rotate+1, len(nums)-1)
```
哈，比nn上一个恶心的代码更恶心的是nn的下一个代码，想不到吧！
#### c++ code[4ms 8,7MB]
```
class Solution {
public:
    bool search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size()-1;
        if (nums.size()==0)
        {
            return false;
        }
        int mark = nums[left];
        if (nums[left]==nums[right])
        {
            if (nums[left]==target)
            {
                return true;
            }
            else{
                while (left<=right && nums[left]==mark)
                {
                    left = left + 1;
                }
                while (right>=0 && nums[right]==mark)
                {
                    right = right - 1;
                }
                if (right == -1)
                    return false;
                left = left-1;
            }
        }
        return search_nodup(nums, target, left, right);
    }
    bool search_nodup(vector<int>& nums, int target,int left, int right) {
        while (left<right)
        {
            int mid = (left+right)/2;
            if (target >= nums[0]) //left
            {
                if(nums[mid]<target && nums[mid]>=nums[0])
                    left = mid + 1;
                else
                    right = mid;
            }
            else{
                if(nums[mid]>=target && nums[mid]<nums[0])
                    right = mid;
                else
                    left = mid + 1;
            }
        }
        if (nums[left]==target)
        {
            return true;
        }
        else
            return false;
    }
};
```
#### code from idol
```
/*
 * @lc app=leetcode id=81 lang=cpp
 *
 * [81] Search in Rotated Sorted Array II
 */
#include <algorithm>
#include <vector>
using std::vector;

class Solution {
 public:
  bool search(vector<int> &nums, int target) {
    return search(nums, 0, nums.size() - 1, target);
  }

  bool search(const vector<int> &nums, int l, int r, int target) {
    if (l > r) {
      return false;
    }
    if (nums[l] < nums[r]) {
      return *std::lower_bound(&nums[l], &nums[r], target) == target;
    }
    if (nums[l] > nums[r]) {
      if (nums[l] > target && target > nums[r]) {
        return false;
      }
    }

    int mid = (l + r) / 2;
    if (nums[mid] == target) {
      return true;
    }
    return search(nums, l, mid - 1, target) || search(nums, mid + 1, r, target);
  }
};
```
好啊，不愧是他。为啥人家就不用考虑重复的情况就考虑进去了重复的情况啊。这咋寻思出来的啊。
上火。
反正，就一步步来呗，一个个wrong case去解决呗，锻炼一下耐心
"大道且漫漫 咱一步一步走完 宛如那棋谱 总得一步步参"
晚安！
