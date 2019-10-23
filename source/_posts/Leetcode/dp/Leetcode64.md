---
title: Leetcode64:Minimum Path Sum
tags: code
comment: true
date: 2019-10-23 12:09:00
---
### 题目描述
Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.
Note: You can only move either down or right at any point in time.
Example:
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.

### 解
sum[i][j]=min(sum[i-1][j]+grid[i][j],sum[i][j-1]+grid[i][j])
#### python code[132ms 16.1MB]
```
class Solution:
    def minPathSum(self, grid: List[List[int]]) -> int:
        row_length=len(grid)
        column_length=len(grid[0])
        sum = [[float('inf') for _ in range(column_length+1)] for _ in range(row_length+1)]
        for i in range(1, row_length+1):
            for j in range(1, column_length+1):
                if i==1 and j==1:
                    sum[i][j]=grid[i-1][j-1]
                else:
                    sum[i][j]=min(sum[i-1][j]+grid[i-1][j-1],sum[i][j-1]+grid[i-1][j-1])
        return sum[-1][-1]
```
#### c++ code[16ms 11MB]
```
#define MAX_INT 10000000
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int row_length=grid.size();
        int column_length=grid[0].size();
        vector<int> row(column_length+1,MAX_INT);
        vector<vector<int>> sum(row_length+1,row);
        for(int i=1;i<row_length+1;i++){
            for (int j = 1; j < column_length+1; j++)
            {
                if(i==1&&j==1) sum[i][j]=grid[i-1][j-1];
                else sum[i][j]=min(sum[i-1][j]+grid[i-1][j-1],sum[i][j-1]+grid[i-1][j-1]);
            }
        }
        return sum[row_length][column_length];
    }
};
```
