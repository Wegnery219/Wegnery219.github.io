---
title: Hash
tags: Data Structure
comment: true
date: 2019-09-08 12:09:00
description: 手写两种hash table:open hash,close hash.
---
哈希是一种经典的数据结构，通过额外的一个数组空间，实现O(1)的查找。 查找迅速，但是带来的缺点是hash table的长度如果过长，整体占用空间会很大，因此提出了散列函数，用取余，随机，定义函数等方式将一个数映射到一个地址空间，如果存在了散列冲突，解决方法分为openhash,closehash。
[wiki链接](https://zh.wikipedia.org/wiki/哈希表)
#### open hash
开放定址，一个数最后被散列到的地址空间不固定，不能定义erase操作。
```
const int N = 1e3; //length
class OpenHash{
    public:
        OpenHash(){
            memset(hash,0,sizeof(hash));
        }
        void Set(int n){
            int p = (n % N + N) % N;
            while (hash[p] && value[p]!=n)
            {
                p = (p+1)%N;
            }
            hash[p] = true;
            value[p] = n;
        }

        bool Get(int n){
            int p = (n % N + N) % N;
            while (hash[p] && value[p]!=n)
            {
                p = (p+1) % N; // 不会死循环，因为hash table比原始的要长，总会有空的。
            }
            return hash[p] && value[p]==n;
        }

    private:
        bool hash[N];
        int value[N];
};
```
#### close hash 
散列地址固定，但是hash table中存的是数组。
##### set
set是一种红黑树的结构，如果N过大建立N个红黑树的开销会很大，
```
const int N = 1e3;
class CloseHash{
    public:
        void Set(int n){
            int p = (n % N + N) % N;
            hash[p].insert(n);
        }
        bool Get(int n){
            int p = (n % N + N) % N;
            return hash[p].count(n);
        }
        void Remove(int n){
            int p = (n % N + N) % N;
            if (hash[p].count(n))
            {
                hash[p].erase(n);
            }
        }
    private:
        set<int> hash[N];
};
```
#### vector
```
const int N = 1e3;
class CloseHash_vec{
    public:
        void Set(int n){
            int p = (n % N + N) % N;
            bool found = false;
            for(int nums:hash[p]){
                if(nums==n){
                    found = true;
                    break;
                }
            }
            if(found==false)
                hash[p].push_back(n);
        }
        bool Get(int n){
            int p = (n % N + N) % N;
            for (int num:hash[p])
            {
                if (num == n)
                {
                    return true;
                }
            }
            return false;
        }
        void Remove(int n){
            int p = (n % N + N) % N;
            for(int i=0; i<hash[p].size();i++){
                if (hash[p][i] == n)
                {
                    hash[p].erase(hash[p].begin()+i);
                    break;
                }
            }
        }
    private:
        vector<int> hash[N];
};
```
