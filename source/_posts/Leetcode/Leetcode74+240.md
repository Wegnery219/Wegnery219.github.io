---
title: Leetcode74+240:Search a 2D Matrix I&II
tags: code
comment: true
date: 2019-09-03 12:09:00
---
## Leetcode 74
### 题目描述
编写一个高效的算法来判断 m x n 矩阵中，是否存在一个目标值。该矩阵具有如下特性：

每行中的整数从左到右按升序排列。
每行的第一个整数大于前一行的最后一个整数。

示例 1:

输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
输出: true

示例 2:

输入:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
输出: false

[链接：https://leetcode-cn.com/problems/search-a-2d-matrix](https://leetcode-cn.com/problems/search-a-2d-matrix)
### 解
每一行的第一个整数大于前一行的最后一个整数，所以先用二分查找找出target在哪一行，然后在找出在这一行的具体位置。写的时候不要忘记判断数组为空的情况。
#### python code [60ms 13.5MB]
```
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if matrix == [] or matrix == [[]]:
            return False
        row = len(matrix)
        print(row)
        column = len(matrix[0])
        print(column)
        if row==0 and column==0:
            return False
        left = 0
        if row>1:
            right = row - 1
        else:
            right = 0
        while left<=right:
            mid = (left+right) // 2
            if matrix[mid][0] == target:
                return True
            elif matrix[mid][0]>target:
                right = mid - 1
            else:
                left = mid + 1
        if right<0:
            return False
        else:
            fix = right
        begin = 0
        if column==1:
            return False
        else:
            end = column - 1
        while begin<=end:
            mid = (begin+end) // 2
            print(mid)
            if matrix[fix][mid] == target:
                return True
            elif matrix[fix][mid]>target:
                end = mid - 1
            else:
                begin = mid + 1
        return False
```
#### C++ code [16ms 9.5MB]
```
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if(matrix.size()==0 || matrix[0].size()==0){
            return false;
        }
        int row = matrix.size();
        int column = matrix[0].size();
        int left = 0;
        int right = row - 1;
        while (left <= right)
        {
            int mid = (left+right)/2;
            if(matrix[mid][0] == target){
                return true;
            }
            else if (matrix[mid][0] > target){
                right = mid - 1;
            }
            else
            {
                left = mid + 1;
            }
        }
        if(right<0){
            return false;
        }
        int fix = right;
        int begin = 0;
        int end = column - 1;
        while (begin <= end)
        {
            int mid = (begin+end)/2;
            if(matrix[fix][mid] == target){
                return true;
            }
            else if(matrix[fix][mid] > target){
                end = mid - 1;
            }
            else
            {
                begin = mid + 1;
            }
        }
        return false;
    }
};
```
## Leetcode 240
### 题目描述
编写一个高效的算法来搜索 m x n 矩阵 matrix 中的一个目标值 target。该矩阵具有以下特性：

每行的元素从左到右升序排列。
每列的元素从上到下升序排列。
示例:

现有矩阵 matrix 如下：

[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
给定 target = 5，返回 true。

给定 target = 20，返回 false。

[链接：https://leetcode-cn.com/problems/search-a-2d-matrix-ii](https://leetcode-cn.com/problems/search-a-2d-matrix-ii)
### 解
和74不一样的是它是每列的元素从上到下升序，所以就不能用大于一个数来判断它在哪里，因为也可能在右上角的矩阵框里。所以想法就是用四个角中的一个数作为起始，逐渐缩小空间，直到缩小为一个数。左上角和右下角的数是不能作为起始的，因为就算确定了和target的大小关系，也不能缩小搜索空间。只能用右上角和左下角的数。
#### python code [124ms 15.8MB]
```
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if matrix == [] or matrix == [[]]:
            return False
        column = len(matrix[0])
        return self.search(matrix, 0, column-1, target)
    
    def search(self, matrix, row, column, target):
        if row>=len(matrix) or column<0:
            return False
        if matrix[row][column] == target:
            return True
        elif matrix[row][column] > target:
            return self.search(matrix, row, column-1, target)
        else:
            return self.search(matrix, row+1, column, target)
```
因为调用函数会浪费时间，所以改用循环。
#### python code [64ms 15.6MB]
```
class Solution(object):
    def searchMatrix(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if matrix == [] or matrix == [[]]:
            return False
        row = len(matrix)
        column = len(matrix[0])
        irow = 0
        icolumn = column - 1
        while irow<row and icolumn>=0:
            if matrix[irow][icolumn] == target:
                return True
            elif matrix[irow][icolumn] > target:
                icolumn -= 1
            else:
                irow += 1
        return False
```
#### C++ code [80ms 12.7MB]
```
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        if (matrix.size()==0 || matrix[0].size()==0)
        {
            return false;
        }
        int row = matrix.size();
        int column = matrix[0].size();
        int irow = 0;
        int icolumn = column - 1;
        while (irow<row && icolumn>=0)
        {
            if(matrix[irow][icolumn] == target){
                return true;
            }
            else if (matrix[irow][icolumn] > target)
            {
                icolumn -= 1;
            }
            else
            {
                irow += 1;
            }
            
        }
        return false;
    }
};
```
