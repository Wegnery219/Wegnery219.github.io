---
title: 1140. Stone Game II
tags: code
comment: true
date: 2019-11-8 12:09:00
---
### [题目描述](https://leetcode.com/problems/stone-game-ii/)
Alex and Lee continue their games with piles of stones.  There are a number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].  The objective of the game is to end with the most stones. 
Alex and Lee take turns, with Alex starting first.  Initially, M = 1.
On each player's turn, that player can take all the stones in the first X remaining piles, where 1 <= X <= 2M.  Then, we set M = max(M, X).
The game continues until all the stones have been taken.
Assuming Alex and Lee play optimally, return the maximum number of stones Alex can get.

Example 1:
Input: piles = [2,7,9,4,4]
Output: 10
Explanation:  If Alex takes one pile at the beginning, Lee takes two piles, then Alex takes 2 piles again. Alex can get 2 + 4 + 4 = 10 piles in total. If Alex takes two piles at the beginning, then Lee can take all three piles left. In this case, Alex get 2 + 7 = 9 piles in total. So we return 10 since it's larger. 

Constraints:
1 <= piles.length <= 100
1 <= piles[i] <= 10 ^ 4
### 解
翻译过来就是这次取石头，只从头取，但是每个人取的个数可以不一样，但是有限制。继续用状态，操作的方法分析。
状态：当前拿石头的起始index，M的值，目前是谁拿
操作：这个人拿了几次
一开始错误把拿了几次想当作状态一并处理了，其实这样应该也可以，但是要再加一个数组维度记录，浪费空间。和[stone game](https://wegnery219.github.io/2019/11/06/Leetcode/dp/Leetcode877/)想采取同样的做法，这道题，我们能不能把目前是谁拿一样给去掉呢？答案是可以的。这样最后返回的是`count(Alex)-count(Lee)`,而`count(Alex)+count(Lee)`的值就是`sum(piles)`，由公式可以看出`[count(Alex)-count(Lee)+count(Alex)+count(Lee)]/2=count(Alex)`。
接下来是转移方程：
```
// x是当前这个人拿了几块石头
dp[i][M]=max(piles[begin]+...+piles[begin+x-1]-dp[begin+x][max(x,M)])
```
#### c++ code[8ms 9MB]
递归做法
```
class Solution {
public:
    int sumfunc(vector<int>& arr, int begin, int end){
        if(end>=arr.size()) return -0x7fffffff;
        int sum=0;
        for(int i=begin;i<=end;i++) sum+=arr[i];
        return sum;
    }
    int dp(vector<int>& piles, vector<vector<int>>& record,int begin, int M){
        int length=piles.size();
        
        if(begin>=length) return 0;

        if(record[begin][M]!=0x7fffffff) return record[begin][M];

        int mvalue=-0x7fffffff;
        for(int i=1;i<=2*M;i++){
            
            int sum=sumfunc(piles, begin, begin+i-1);

            int newM;
            if(i>M) newM=i;
            else newM=M;
            mvalue=max(mvalue, sum-dp(piles, record, begin+i, newM));
        }

        record[begin][M]=mvalue;
        return record[begin][M];
    }
    int stoneGameII(vector<int>& piles) {
        vector<vector<int>> record(piles.size(),vector<int>(piles.size(), 0x7fffffff));
        int diff=dp(piles, record, 0, 1);
        int add=sumfunc(piles,0,piles.size()-1);
        
        return (diff+add)/2;
    }
};
```
WA:之前有一次指针错误，原因是在sumfunc中没有判断end和arr.size()的关系，如果end超出arr.size()应该返回一个特别小的值，这样在`max(mvalue, sum-dp(piles, record, begin+i, newM))`中就可以返回mvalue了，就是合法的那个值。

循环做法待我复习一下操作系统再写。
