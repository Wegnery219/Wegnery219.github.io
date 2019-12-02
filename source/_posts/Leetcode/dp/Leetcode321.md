---
title: Leetcode321.Create Maximum Number
date: 2019-12-02 22:45:59
tags: code
---
### [题目描述](https://leetcode.com/problems/create-maximum-number/)
Given two arrays of length m and n with digits 0-9 representing two numbers. Create the maximum number of length k <= m + n from digits of the two. The relative order of the digits from the same array must be preserved. Return an array of the k digits.

Note: You should try to optimize your time and space complexity.

Example 1:

Input:
nums1 = [3, 4, 6, 5]
nums2 = [9, 1, 2, 5, 8, 3]
k = 5
Output:
[9, 8, 6, 5, 3]
Example 2:

Input:
nums1 = [6, 7]
nums2 = [6, 0, 4]
k = 5
Output:
[6, 7, 6, 0, 4]
Example 3:

Input:
nums1 = [3, 9]
nums2 = [8, 9]
k = 3
Output:
[9, 8, 9]
### 解
1. TLE做法，时间复杂度为O(n^3)，根据之前的状态和选择的分析方法。
状态: nums1,nums2中目前含有几个数，k目前是几
选择: 选择哪一个数加入最终数组中返回的数最大
状态转移方程:
```
dp[i][j][k]=max(dp[i-1][j][k-1]*10+nums1[i-1],
                dp[i][j-1][k-1]*10+nums2[j-1],
                dp[i-1][j-1][k],
                dp[i-1][j][k],
                dp[i][j-1][k])
```
初始化: 
① 当k=0时，dp[i][j][k]=0
② 当i+j<k时，dp[i][j][k]=-inf
③ 当i=0或j=0时，dp[i][j][k]等于在其中不等于0的数组中做选择的值
#### python code[TLE 81/102 test cases passed]
```
class Solution:
    def maxNumber(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        dp = [[[0 for _ in range(k+1)] for _ in range(len(nums2)+1)] for _ in range(len(nums1)+1)]
        # init
        for j in range(len(nums2)+1):
            for k in range(1, k+1):
                if j<k:
                    dp[0][j][k]=float('-inf')
                else:
                    dp[0][j][k]=max(dp[0][j-1][k-1]*10+nums2[j-1], dp[0][j-1][k])

        for i in range(len(nums1)+1):
            for k in range(1,k+1):
                if i<k:
                    dp[i][0][k]=float('-inf')
                else:
                    dp[i][0][k]=max(dp[i-1][0][k-1]*10+nums1[i-1], dp[i-1][0][k])
        # dp begin
        for i in range(1, len(nums1)+1):
            for j in range(1, len(nums2)+1):
                for k in range(1,k+1):
                    if (i+j)<k:
                        dp[i][j][k]=float('-inf')
                    else:
                        dp[i][j][k]=max(dp[i-1][j][k-1]*10+nums1[i-1],
                                        dp[i][j-1][k-1]*10+nums2[j-1],
                                        dp[i-1][j-1][k],
                                        dp[i-1][j][k],
                                        dp[i][j-1][k])
        return [int(d) for d in str(dp[-1][-1][-1])]
```
2. 通过了的做法, 对nums1和nums2分别建立一个数组用来存当最终有k个数时它们自己能返回的最大的数是多少，然后再枚举,跟上面的③的想法差不多。时间复杂度为O(n^2)。
#### python code[332ms 13MB]
```
class Solution:
    def maxNumber(self, nums1: List[int], nums2: List[int], k: int) -> List[int]:
        i = 0
        res=[]
        for i in range(k+1):
            if i<=len(nums1) and (k-i)<=len(nums2):
                res=max(res, self.merge(self.getone(nums1, i), self.getone(nums2, k-i)))
        return res

    def merge(self, nums1: List[int], nums2: List[int]) -> List[int]:
        res = []
        while nums1 and nums2:
            if nums1>nums2: # compare the first element in list
                res.append(nums1.pop(0))
            else:
                res.append(nums2.pop(0))
        res = res + nums1 + nums2
        return res
    
    def getone(self, nums: List[int], k: int) -> List[int]:
        res = []
        ind = len(nums)-k # use to control number<k
        for i in range(len(nums)):
            while len(res)!=0 and res[-1]<nums[i] and ind:
                ind -= 1
                res.pop()
            res.append(nums[i])
        return res[:k]
```
