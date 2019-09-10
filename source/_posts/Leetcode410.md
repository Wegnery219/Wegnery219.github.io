---
title: Leetcode410
tags: code
comment: true
date: 2019-09-10 12:09:00
---
### 题目描述：
给定一个非负整数数组和一个整数 m，你需要将这个数组分成 m 个非空的连续子数组。设计一个算法使得这 m 个子数组各自和的最大值最小。
注意:
数组长度 n 满足以下条件:
1 ≤ n ≤ 1000
1 ≤ m ≤ min(50, n)
示例:
输入:
nums = [7,2,5,10,8]
m = 2
输出:
18
解释:
一共有四种方法将nums分割为2个子数组。
其中最好的方式是将其分为[7,2,5] 和 [10,8]，
因为此时这两个子数组各自的和的最大值为18，在所有情况中最小。

[链接：https://leetcode-cn.com/problems/split-array-largest-sum](https://leetcode-cn.com/problems/split-array-largest-sum)
### 解
和Leetcode378类似，在所有解空间内通过`m个子数组`这个条件二分猜解，所有的解空间是数组中最大的数到所有数组的和，如果当前count[mid]<=m,说明还要继续分数组，则说明和的最大值的最小值比当前mid还要小，在min和mid之间继续二分。
#### python code[20ms 11.7MB]
(第一个100%哦嘻嘻
```
class Solution(object):
    def bisect_count(self, nums, left, mid, right):
        count = 0
        total = 0
        for num in nums:
            total += num
            if total>mid:
                count += 1
                total = num
        return count + 1
    def splitArray(self, nums, m):
        """
        :type nums: List[int]
        :type m: int
        :rtype: int
        """
        left = max(nums)
        right = sum(nums)
        while left<=right:
            mid = (left+right)//2
            count = self.bisect_count(nums,left,mid,right)
            if count<=m:
                right = mid - 1
            else:
                left = mid + 1
        return left
```
#### c++ code[4ms 8.4MB]
```
class Solution {
public:
    int bisect_count(vector<int>& nums, long left, long mid,long right){
        int count = 0;
        long total = 0;
        for (auto num:nums)
        {
            total += num;
            if (total>mid)
            {
                count += 1;
                total = num;
            }
        }
        return count + 1;
    }
    int splitArray(vector<int>& nums, int m) {
        long left = 0;
        long right = 0;
        for (auto num:nums)
        {
            right += num;
            if (left<num)
            {
                left = num;
            }
        }
        while (left<right)
        {
            long mid = (left+right)/2;
            int count = bisect_count(nums, left, mid, right);
            if (count<=m)
            {
                right = mid;
            }
            else
            {
                left = mid + 1;
            }
        }
        return left;
    }
};
```
### 二分查找的两种方式
#### 1. `<`
```
int min=0;
int max=size;
while(min<max){
    int mid=(min+max)/2;
    if (a[mid]>=m)
    {
        max=mid;
    }
    else{
        min=mid+1;
    }
}
return min;
```
用这种二分的方式max可以等于mid，然后max要等于size，不然最后一个元素会访问不到。
#### 2. `<=`
```
int min=0;
int max=size-1;
while(min<=max){
    int mid = (min+max)/2;
    if (a[mid]>=m)
    {
        max = mid - 1;
    }
    else{
        min = mid + 1;
    }
}
return min;
```
嘻嘻我一般用后面这个
