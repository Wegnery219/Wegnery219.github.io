---
title: Best Time to Buy and Sell Stock I&II
tags: code
comment: true
date: 2019-10-30 12:09:00
---
## Leetcode121.Best Time to Buy and Sell Stock
### 题目描述
Say you have an array for which the ith element is the price of a given stock on day i.
If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock),design an algorithm to find the maximum profit.
Note that you cannot sell a stock before you buy one.
Example 1:
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5. 7-1 = 6, as selling price needs to be larger than buying price.
Example 2:
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
### 解
#### python code[80ms 14.8MB]
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        if len(prices)==0:
            return 0
        max_profit = 0
        min_price = prices[0]
        for i in range(1,len(prices)):
            max_profit = max(prices[i]-min_price, max_profit)
            min_price = min(min_price, prices[i])
        return max_profit
```
#### c++ code[4ms 9.6MB]
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()==0) return 0;
        int max_profit = 0;
        int min_price = prices[0];
        for (int i = 1; i < prices.size(); i++)
        {
            max_profit = max(prices[i]-min_price, max_profit);
            min_price = min(min_price, prices[i]);
        }
        return max_profit;
    }
};
```
## Leetcode122.Best Time to Buy and Sell Stock II
### 题目描述
Say you have an array for which the ith element is the price of a given stock on day i.

Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).

Note: You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).
Example 1:
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
Example 2:
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.Note that you cannot buy on day 1, buy on day 2 and sell them later, as you areengaging multiple transactions at the same time. You must sell before buying again.
### 解
动态规划做法：
到第i天的时候，分为两种状态，0.没有股票 1.持有股票，这两种状态可以从第i-1天的状态转移而来，具体方法为：
```
第i天没有股票：1)i-1天如果有股票，i天需要卖掉 
            2)i-1天如果没有股票，不做操作
第i天有股票:1)i-1天如果没有股票，i天要买入
          2)i-1天如果有股票，i天不做操作
```
#### c++ code[12ms 11.6MB]
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()==0) return 0;
        vector<int> a(2,0);
        vector<vector<int>> record(prices.size(), a);

        record[0][0]=0;
        record[0][1]=-prices[0];
        for(int i=1;i<prices.size();i++){
            record[i][0] = max(record[i-1][1]+prices[i], record[i-1][0]);
            record[i][1] = max(record[i-1][1], record[i-1][0]-prices[i]);
        }
        return max(record[prices.size()-1][1], record[prices.size()-1][0]);
    }
};
```
做的时候记得考虑数组是空的情况有没有cover。看代码发现其实上面record数组是多余的，因为第i天只和第i-1天有关，用两个变量代替一下。
#### c++ code[8ms 9.6MB]
```

class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if(prices.size()==0) return 0;

        int zero=0;
        int one=-prices[0];
        for(int i=1;i<prices.size();i++){
            zero = max(one+prices[i], zero);
            one = max(one, zero-prices[i]);

            zero = zero;
            one = one;
        }
        return max(one, zero);
    }
};
```
看了题解，这道题还可以用贪心，贪心做法就是把股票的图画成一个折线图，
![image](https://leetcode.com/media/original_images/122_maxprofit_1.PNG)
一定是在拐点(极小值点)买入，否则的话可以反证，任意取一个非极小值点买入，如果在离这个点最近的极小值点买入，一定可以获得更高的利润。
反过来说就是在非下降的拐点一直往上加就好了。
#### c++ code[8ms 9.6MB]
```
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int max_profit=0;

        for(int i=1; i<prices.size();i++){
            if(prices[i]>prices[i-1]){
                max_profit += prices[i]-prices[i-1];
            }
        }
        return max_profit;
    }
};
```
#### python code[80ms 14.8MB]
```
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        max_profit=0

        for i in range(1,len(prices)):
            if prices[i]>prices[i-1]:
                max_profit += prices[i]-prices[i-1]
        
        return max_profit
```
嗐，会了贪心再看自己的动归好智障。
