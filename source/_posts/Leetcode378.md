---
title: Leetcode378
tags: code
comment: true
date: 2019-09-09 12:09:00
---
### 题目描述：
给定一个 n x n 矩阵，其中每行和每列元素均按升序排序，找到矩阵中第k小的元素。
请注意，它是排序后的第k小元素，而不是第k个元素。
示例:
matrix = [
   [ 1,  5,  9],
   [10, 11, 13],
   [12, 13, 15]
],
k = 8,
返回 13。
说明: 
你可以假设 k 的值永远是有效的, 1 ≤ k ≤ n2 。

[链接：https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix](https://leetcode-cn.com/problems/kth-smallest-element-in-a-sorted-matrix)
### 解：
1. 用堆来存储矩阵中的元素，但是没有利用到矩阵的大小关系，时间复杂度O(N*log(N))
#### c++ code[108ms 13.3MB]
```
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        int n = matrix.size();
        priority_queue<int> myq;
        for (int i = 0;i < n; i++)
        {
            for (int j = 0; j < n; j++)
            {
                myq.push(matrix[i][j]);
            }
        }
        while (myq.size()>k)
        {
            myq.pop();
        }
        return myq.top();
    }
};
```
2. 和Leetcode240一样，在240中是找到矩阵中是否存在target的值，在找target值的时候可以根据每次缩小的范围找到比target小的矩阵中item的数量。**数量->找到第k小元素:**,在matrix的最大和最小值之间二分猜解，通过二分找到`count[ret]>=k`的最小值ret。时间复杂度为O(N).
证明ret一定在矩阵中：
```
count[ret]>=k   //ret是满足该不等式的minum和maxium之间的最小值
count[ret] <==> matrix 中小于等于ret的数量
如果ret不在矩阵中：
count[ret] <==> matrix 中小于ret的数量
则count[ret-1]>=k  
==> ret-1 是满足该不等式的最小值，矛盾。
```
#### python code[268ms 17.1MB]
```
class Solution(object):
    def kthSmallest(self, matrix, k):
        """
        :type matrix: List[List[int]]
        :type k: int
        :rtype: int
        """
        n = len(matrix)
        minum = matrix[0][0]
        maxium = matrix[n-1][n-1]
        count = 0
        while minum<=maxium:
            i=0
            j=n-1
            count = 0
            mid = (minum + maxium) // 2
            while i<n and j>=0:
                if matrix[i][j] <= mid:
                    count = count + j + 1
                    i = i + 1
                else:
                    j = j - 1
            if count >= k:
                maxium = mid - 1
            else:
                minum = mid + 1
        return minum
```
#### c++ code[72ms 11.9MB]
```
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
        long minum = matrix[0][0];
        int l = matrix.size();
        long maxium = matrix[l-1][l-1];
        while (minum <= maxium)
        {
            int count = 0;
            long mid = (minum+maxium)/2;
            int i = 0;
            int j = l - 1;
            while (i<l && j>=0)
            {
                if (matrix[i][j]<=mid)
                {
                    count = count + j + 1;
                    i = i + 1;
                }
                else
                {
                    j = j - 1;
                }
            }
            if (count >= k)
            {
                maxium = mid - 1;
            }
            else
            {
                minum = mid + 1;
            }
        }
        return minum;
    }
};
```
