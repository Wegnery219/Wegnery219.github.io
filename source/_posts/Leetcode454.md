---
title: Leetcode454:4Sum II
tags: code
comment: true
date: 2019-09-11 11:11:00
---
### 题目描述：
给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。
例如:
输入:
A = [ 1, 2]
B = [-2,-1]
C = [-1, 2]
D = [ 0, 2]
输出:
2
解释:
两个元组如下:
1. (0, 0, 0, 1) -> A[0] + B[0] + C[0] + D[1] = 1 + (-2) + (-1) + 2 = 0
2. (1, 1, 0, 0) -> A[1] + B[1] + C[0] + D[0] = 2 + (-1) + (-1) + 0 = 0

[链接：https://leetcode-cn.com/problems/4sum-ii](https://leetcode-cn.com/problems/4sum-ii)
### 解：
a+b+c+d=0 <==> a+b=-(c+d),可以将求出的和放进hash table，然后另两个元素求和的时候在hash table中找。
#### python code[300ms 34.1MB]
```
class Solution(object):
    def fourSumCount(self, A, B, C, D):
        """
        :type A: List[int]
        :type B: List[int]
        :type C: List[int]
        :type D: List[int]
        :rtype: int
        """
        length = len(A)
        apb = {}
        for a in A:
            for b in B:
                he = a+b
                if he in apb:
                    apb[he]+=1
                else:
                    apb[he]=1
        count = 0
        for c in C:
            for d in D:
                he = c + d
                if -he in apb:
                    count += apb[-he]
        return count
```
#### c++ code[260ms 28.5MB]
```
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int,int> hash_ab;
        for (auto a:A)
        {
            for(auto b:B){
                int he = a + b;
                if (hash_ab.count(he))
                {
                    hash_ab[he] += 1;
                }
                else
                {
                    hash_ab[he] = 1;
                }
            }
        }
        int count=0;
        for(auto c:C){
            for(auto d:D){
                int he = c + d;
                if(hash_ab.count(-he))
                    count += hash_ab[-he];
            }
        }
        return count;
    }
};
```
#### c++ code[112ms 16.4MB]
(我的第二个100%乌啦啦啦啦
```
const int N = 1e6;
class Hash{
    public:
        Hash(){
            memset(hash, 0, sizeof(hash));
        }
        void Set(int n){
            int p = (n % N + N) % N;
            while (hash[p]!=0 && value[p]!=n)
            {
                p = (p+1)%N;
            }
            hash[p] += 1;
            value[p] = n;
        }
        int Get(int n){
            int p = (n % N + N) % N;
            while (hash[p]!=0 && value[p]!=n)
            {
                p = (p + 1) % N;
            }
            if (hash[p]==0)
                return -1;
            else
                return hash[p];
        }
    private:
        int hash[N];
        int value[N];
};
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        Hash saved;
        for(auto &a:A){
            for(auto &b:B){
                saved.Set(a+b);
            }
        }
        int count = 0;
        for(auto &c:C){
            for(auto &d:D){
                int rt = saved.Get(-c-d);
                if(rt!=-1)
                    count += rt;
            }
        }
        return count;
    }
};
```
#### c++ code from idol[48ms 36.1MB]
加了clear函数以后hash就可以反复用啦，缺点是tick有限，所以到最后了会崩，虽然爱豆的比我快但我也是100%哦，比我快的原因是没有用memset.
```
#include <vector>
#include <unordered_map>
#include <iostream>
using namespace std;
const int N = 2333333;

class Hash {
 public:
  Hash() { tick = 1; }
  void Clear() { ++tick; }
  void Set(int n) {
    int p = (n % N + N) % N;
    while (hash[p] == tick && origin[p] != n) {
      p = (p + 1) % N;
    }
    if (hash[p] != tick) {
      hash[p] = tick;
      origin[p] = n;
      value[p] = 1;
    } else {
      ++value[p];
    }
  }
  int Get(int n) {
    int p = (n % N + N) % N;
    while (hash[p] == tick && origin[p] != n) {
      p = (p + 1) % N;
    }
    return hash[p] == tick ? value[p] : 0;
  }

 private:
  int tick;
  int hash[N];
  int origin[N];
  int value[N];
} saved;

class Solution {
 public:
  int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C,
                   vector<int>& D) {
    saved.Clear();
    for (auto& a : A) {
      for (auto& b : B) {
        saved.Set(a + b);
      }
    }
    int count = 0;
    for (auto& c : C) {
      for (auto& d : D) {
        count += saved.Get(-c - d);
      }
    }
    return count;
  }
};
```
