---
title: 877. Stone Game
tags: code
comment: true
date: 2019-11-6 12:09:00
---
### [题目描述](https://leetcode.com/problems/stone-game/)
Alex and Lee play a game with piles of stones.  There are an even number of piles arranged in a row, and each pile has a positive integer number of stones piles[i].
The objective of the game is to end with the most stones.  The total number of stones is odd, so there are no ties.
Alex and Lee take turns, with Alex starting first.  Each turn, a player takes the entire pile of stones from either the beginning or the end of the row.  This continues until there are no more piles left, at which point the person with the most stones wins.
Assuming Alex and Lee play optimally, return True if and only if Alex wins the game.

Example 1:
Input: [5,3,4,5]
Output: true
Explanation: 
Alex starts first, and can only take the first 5 or the last 5.
Say he takes the first 5, so that the row becomes [3, 4, 5].
If Lee takes 3, then the board is [4, 5], and Alex takes 5 to win with 10 points.
If Lee takes the last 5, then the board is [3, 4], and Alex takes 4 to win with 9 points.
This demonstrated that taking the first 5 was a winning move for Alex, so we return true.
Note:
2 <= piles.length <= 500
piles.length is even.
1 <= piles[i] <= 500
sum(piles) is odd.
### 解
状态:当前取数的数组(从i开始j结束),有一个[题解](https://leetcode-cn.com/problems/stone-game/solution/jie-jue-bo-yi-wen-ti-de-dong-tai-gui-hua-tong-yong/)是把参与博弈的人也算作状态，这里也可以这样做，但是两个人都一样聪明，所以可以就假设只有一个人，这个人在i,j时候做出来的选择一定是和另一个人是一样的。
操作:操作是取开头的数，或者取结尾的数
状态转移方程：
```
dp[i][j]=max(piles[i]-dp[i+1][j],piles[j]-dp[i][j-1]) if i!=j
dp[i][j]=piles[i] if i==j
```
base condition:
```
dp[-1][j],dp[i][length+1],这种情况应该是非法的吧(?),然后所以应该初始化成MAXINT?
```
目前用循环的写法还没过。
#### c++ code[60ms 74.9MB]
```
#define MAXINT 0x7fffffff
class Solution {
public:
    int dp(vector<int> &piles, vector<vector<int>> &record, int i, int j){
        if(record[i][j]!=MAXINT) return record[i][j];
        if(i==j) {record[i][j]=piles[i];return piles[i];}
        else {
            record[i][j]=max(piles[i]-dp(piles, record, i+1, j), piles[j]-dp(piles, record, i, j-1));
            return record[i][j];
        }
    }
    bool stoneGame(vector<int>& piles) {
        vector<vector<int>> record(501, vector<int> (501,MAXINT));
        if(dp(piles, record, 0, piles.size()-1)>0) return true;
        else return false;
    }
};
```
还有一种做法就是
```
class Solution {
public:
    bool stoneGame(vector<int>& piles) {
        return true;
    }
};
```
举个例子，现在有A,B,C,D四堆，alex先手，那么他可以取(A,C),(A,B),(D,A),(B,D),(C,D)。四堆分两堆，会发现上面这几个总有一个和是最大的，也就是不管哪一堆和是最大的，alex总能先取到，只要给alex足够时间枚举，他就可以按顺序取选和最大的那个组合:)，建议玩这种游戏的时候保证石头堆数是奇数哦科科。
