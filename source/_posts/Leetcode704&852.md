---
title: Leetcode704:Binary Search+852:Peak Index in a Mountain Array 
tags: code
comment: true
date: 2019-09-14 14:14:00
---
## Leetcode704
### 题目描述:
给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。
示例 1:
输入: nums = [-1,0,3,5,9,12], target = 9
输出: 4
解释: 9 出现在 nums 中并且下标为 4
示例 2:
输入: nums = [-1,0,3,5,9,12], target = 2
输出: -1
解释: 2 不存在 nums 中因此返回 -1
提示：
你可以假设 nums 中的所有元素是不重复的。
n 将在 [1, 10000]之间。
nums 的每个元素都将在 [-9999, 9999]之间。

[链接：https://leetcode-cn.com/problems/binary-search](https://leetcode-cn.com/problems/binary-search)
### 解：
二分查找
#### c++ code[52ms 10.7MB]
```
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size()-1;
        while(left <= right){
            int mid = (left+right) / 2;
            if(nums[mid]==target){
                return mid;
            }
            else if(nums[mid]>target){
                right = mid - 1;
            }
            else{
                left = mid + 1;
            }
        }
        return - 1;
    }
};
```
## Leetcode852
### 题目描述：
我们把符合下列属性的数组 A 称作山脉：
A.length >= 3
存在 0 < i < A.length - 1 使得A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1]
给定一个确定为山脉的数组，返回任何满足 A[0] < A[1] < ... A[i-1] < A[i] > A[i+1] > ... > A[A.length - 1] 的 i 的值。
示例 1：
输入：[0,1,0]
输出：1
示例 2：
输入：[0,2,1,0]
输出：1
提示：
3 <= A.length <= 10000
0 <= A[i] <= 10^6
A 是如上定义的山脉

[链接：https://leetcode-cn.com/problems/peak-index-in-a-mountain-array](https://leetcode-cn.com/problems/peak-index-in-a-mountain-array)
### 解：
二分查找
#### c++ code[20ms 9.2MB]
```
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& A) {
        int left = 0;
        int right = A.size()-1;
        while(left <= right){
            int mid = (left+right)/2;
            if(A[mid]>A[mid-1] && A[mid]>A[mid+1]){
                return mid;
            }
            else if(A[mid]<A[mid+1] && A[mid]>A[mid-1]){
                left = mid + 1;
            }
            else{
                right = mid - 1;
            }
        }
        return -1;
    }
};
```
中秋快乐！
