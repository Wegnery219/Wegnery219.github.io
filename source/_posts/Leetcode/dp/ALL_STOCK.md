---
title: All Stock
tags: code
comment: true
date: 2019-11-2 12:09:00
---
所有股票题的通用解法参考链接：[link](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-iii/solution/yi-ge-tong-yong-fang-fa-tuan-mie-6-dao-gu-piao-wen/)
使用状态+选择的状态机方法。
状态：第几天(n)，一共允许几次交易(k)，目前手中是否持有股票(0,1)。n个状态就要做n-1次循环
选择：卖股票，不卖股票，不操作。每种状态可以从不同的选择得到。
假设有n天，最后返回的是第n天，第k次交易，没有股票的值，因为假设第n天还有股票，那么卖了所获得的收益肯定比不卖多。
通用状态转移方程：
```
//第i天第k次交易时没有股票的状态可能从两种状态转移来，一种是第i-1天也没有股票，此时做的选择是不操作。一种是第i-1天有股票，此时做的操作是卖掉股票。

dp[i][k][0]=max(dp[i-1][k][0],dp[i-1][k][1]+prices[i])

//第i天第k次交易时有股票的状态可能从两种状态转移来，一种是第i-1天有股票，此时做的选择是不操作，另一种是第i-1天没有股票，此时做的操作是买股票，我们规定买股票的时候对应交易次数要加1，也可以定义成卖股票的时候+1.

dp[i][k][1]=max(dp[i-1][k][1],dp[i-1][k-1][0]-prices[i])

//base condition
//第-1天第k次交易没有股票,没有股票，利润为0
dp[-1][k][0]=0
//第-1天第k次交易有股票,不可能发生
dp[-1][k][1]=-infinity
//第i天第0次交易没有股票,没有利润。注：k从1开始
dp[i][0][0]=0
//第i天第0次交易有股票，没有交易就没有股票，不合法。
dp[i][0][1]=-infinity
```
用这种通用做法重新做了一遍所有的股票题。
### 121. Best Time to Buy and Sell Stock
condition:If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.
意味着k=1，将k=1代入通用状态转移方程。
```
dp[i][1][0]=max(dp[i-1][1][0],dp[i-1][1][1]+prices[i])
dp[i][1][1]=max(dp[i-1][1][1],dp[i-1][0][0]-prices[i])

//根据base condition
dp[i][0][0]=0

//简化之后：
dp[i][0]=max(dp[i-1][0],dp[i-1][1]+prices[i])
dp[i][1]=max(dp[i-1][1],-prices[i])
```
#### c++ code[8ms 11.8MB]
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()==0) return 0;
        vector<int> a(2,0);
        vector<vector<int>> dp(prices.size(),a);

        for(int i=0;i<prices.size();i++){
            if(i==0){
                dp[i][0]=0;
                dp[i][1]=-prices[i];
            }
            else{
                dp[i][0]=max(dp[i-1][0],dp[i-1][1]+prices[i]);
                dp[i][1]=max(dp[i-1][1],-prices[i]);
            }
        }
        return dp[prices.size()-1][0];
    }
};
```
接下来简化空间，注意到dp[i]只和dp[i-1]相关，用一个变量记录一下。
#### c++ code[4ms 9.5MB]
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()==0) return 0;

        int dp_i_0=0;
        int dp_i_1=-prices[0];
        for(int i=1;i<prices.size();i++){
            dp_i_0=max(dp_i_0,dp_i_1+prices[i]);
            dp_i_1=max(dp_i_1,-prices[i]);
        }
        return dp_i_0;
    }
};
```
### 122.Best Time to Buy and Sell Stock II
condition:Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).
意味着k等于无穷大，将k等于∞代入通用状态转移
```
dp[i][k][0]=max(dp[i-1][k][0],dp[i-1][k][1]+prices[i])
dp[i][k][1]=max(dp[i-1][k][1],dp[i-1][k-1][0]-prices[i])

k=∞，意味着k=k-1，则状态转移可以简写为
dp[i][0]=max(dp[i-1][0],dp[i-1][1]+prices[i])
dp[i][1]=max(dp[i-1][1],dp[i-1][0]-prices[i])
```
#### c++ code[8ms 9.5MB]
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()==0) return 0;

        int dp_i_0=0;
        int dp_i_1=-prices[0];

        for(int i=1;i<prices.size();i++){
            int temp=dp_i_0;
            dp_i_0=max(dp_i_0,dp_i_1+prices[i]);
            dp_i_1=max(dp_i_1,temp-prices[i]);
        }

        return dp_i_0;
    }
};
```
### 123. Best Time to Buy and Sell Stock III
condition: Design an algorithm to find the maximum profit. You may complete at most two transactions.
k=2,转移方程就是通用转移方程
```
dp[i][k][0]=max(dp[i-1][k][0],dp[i-1][k][1]+prices[i])
dp[i][k][1]=max(dp[i-1][k][1],dp[i-1][k-1][0]-prices[i])
```
这个时候该多加一重循环了
#### c++ code[40ms 18MB]
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()==0) return 0;

        vector<int> stock(2,0); //whether have stock
        vector<vector<int>> transaction(3, stock); //k starts from 1
        vector<vector<vector<int>>> dp(prices.size()+1, transaction); //i starts from 1

        for (int i = 0; i < prices.size()+1; i++)
        {
            for (int k = 0; k <= 2; k++)
            {
                if(i==0) dp[i][k][0]=0,dp[i][k][1]=-0x7fffffff;
                else if(k==0) dp[i][k][0]=0,dp[i][k][1]=-0x7fffffff;
                else{
                    dp[i][k][0]=max(dp[i-1][k][0],dp[i-1][k][1]+prices[i-1]);
                    dp[i][k][1]=max(dp[i-1][k][1],dp[i-1][k-1][0]-prices[i-1]);
                }
            }
        }
        return dp[prices.size()][2][0];
    }
};
```
枚举k,就不用开数组了
#### c++ code[8ms 9.5MB]
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()==0) return 0;

        int dp_i10=0;int dp_i11=-0x7fffffff;
        int dp_i20=0;int dp_i21=-0x7fffffff;
        for (int i = 0; i < prices.size(); i++)
        {
            dp_i20=max(dp_i20,dp_i21+prices[i]);
            dp_i21=max(dp_i21,dp_i10-prices[i]);
            dp_i10=max(dp_i10,dp_i11+prices[i]);
            dp_i11=max(dp_i11,-prices[i]);
        }
        return dp_i20;
    }
};
```
### 188. Best Time to Buy and Sell Stock IV
留一个给明天嘻嘻
