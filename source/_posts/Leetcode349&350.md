---
title: Leetcode349+350
tags: code
comment: true
date: 2019-09-08 12:09:00
---
## Leetcode349
### 题目描述
给定两个数组，编写一个函数来计算它们的交集。
示例 1:
输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2]
示例 2:
输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [9,4]
说明:
输出结果中的每个元素一定是唯一的。
我们可以不考虑输出结果的顺序。

[链接：https://leetcode-cn.com/problems/intersection-of-two-arrays](https://leetcode-cn.com/problems/intersection-of-two-arrays)
### 解
最初的想法是sort加二分，之后再用个set去重，时间复杂度为O(NlogN),
#### python code[84ms 11.8MB]
```
class Solution(object):
    def intersection(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        nums1.sort()
        res = []
        for i in range(len(nums2)):
            begin = 0
            end = len(nums1)-1
            target = nums2[i]
            while begin<=end:
                mid = (begin+end) // 2
                if nums1[mid] == target:
                    res.append(target)
                    break
                elif nums1[mid] > target:
                    end = mid - 1
                else:
                    begin = mid + 1
        return list(set(res))
```
#### c++ code[12ms 9.1MB]
```
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        if (nums1.size()==0 || nums2.size() == 0)
        {
            return res;
        }
        
        sort(nums1.begin(), nums1.end());
        for (int i=0;i<nums2.size();i++)
        {
            int begin = 0;
            int end = nums1.size()-1;
            int target = nums2[i];
            while (begin <= end)
            {
                int mid = (begin+end) / 2;
                if (nums1[mid] == target)
                {
                    res.push_back(target);
                    break;
                }
                else if(nums1[mid] > target){
                    end = mid - 1;
                }
                else{
                    begin = mid + 1;
                }
            }
        }
        set<int> res1(res.begin(),res.end());
        res.assign(res1.begin(), res1.end());
        return res;
    }
};
```
后来用了python，c++自带的intersection函数自己求交集，学了一下用法。
#### python code[68ms 11.9MB]
```
class Solution(object):
    def intersection(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        set_1 = set(nums1)
        set_2 = set(nums2)
        return list(set_1.intersection(set_2))
```
#### c++ code[12ms 9.3MB]
```
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        sort(nums1.begin(),nums1.end());
        sort(nums2.begin(),nums2.end());
        vector<int> res(nums2.size());
        vector<int> :: iterator it;
        it = set_intersection(nums1.begin(), nums1.end(), nums2.begin(), nums2.end(), res.begin());
        res.resize(it - res.begin());
        set<int> resset(res.begin(),res.end());
        res.assign(resset.begin(),resset.end());
        return res;
    }
};
```
其中c++ set_intersection的源码如下，在后面leetcode350的时候直接用的这个代码嘻嘻。取交集的时候复杂度为O(N)但是因为前面要排序，所以复杂度还是O(NlogN)
[源码链接:http://www.cplusplus.com/reference/algorithm/set_intersection/](http://www.cplusplus.com/reference/algorithm/set_intersection/)
```
template <class InputIterator1, class InputIterator2, class OutputIterator>
  OutputIterator set_intersection (InputIterator1 first1, InputIterator1 last1,
                                   InputIterator2 first2, InputIterator2 last2,
                                   OutputIterator result)
{
  while (first1!=last1 && first2!=last2)
  {
    if (*first1<*first2) ++first1;
    else if (*first2<*first1) ++first2;
    else {
      *result = *first1;
      ++result; ++first1; ++first2;
    }
  }
  return result;
}
```
O(N)的做法是使用hash table，c++的unordered_set,unordered_map,python的dict，set都使用的hash。
#### python code[44ms 11.8MB]
```
class Solution(object):
    def intersection(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        set_1 = set(nums1)
        set_2 = set(nums2)
        return [i for i in set_1 if i in set_2]
```
#### c++ code[12ms 9.2MB]
```
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> saved;
        vector<int> res;
        for(auto num:nums1){
            saved.insert(num);
        }
        for(auto num:nums2){
            if (saved.count(num))
            {
                saved.erase(num);
                res.push_back(num);
            }
        }
        return res;
    }
};
```
后面手写了openhash,closehash(set,vector)分别测试了一下
开放定址法不能remove，如果remove操作会导致整个hash表被打乱。
#### c++ code[12ms 9.1MB openhash]
```
const int N = 1e3;
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

class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        OpenHash saved;
        for (auto num:nums1)
        {
            saved.Set(num);
        }
        for(auto num:nums2){
            if(saved.Get(num)){
                res.push_back(num);
            }
        }
        set<int> res1(res.begin(),res.end());
        res.assign(res1.begin(), res1.end());
        return res;
    }
};
```
#### c++ code[8ms 9.3MB closehash use set]
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
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        CloseHash saved;
        for (auto num:nums1)
        {
            saved.Set(num);
        }
        for(auto num:nums2){
            if(saved.Get(num)){
                res.push_back(num);
                saved.Remove(num);
            }
        }
        return res;
    }
};
```
#### c++ code[12ms 9.1MB closehash use vector]
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
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        CloseHash_vec saved;
        for (auto num:nums1)
        {
            saved.Set(num);
        }
        for(auto num:nums2){
            if(saved.Get(num)){
                res.push_back(num);
                saved.Remove(num);
            }
        }
        return res;
    }
};
```
## Leetcode 350
### 题目描述：
给定两个数组，编写一个函数来计算它们的交集。
示例 1:
输入: nums1 = [1,2,2,1], nums2 = [2,2]
输出: [2,2]
示例 2:
输入: nums1 = [4,9,5], nums2 = [9,4,9,8,4]
输出: [4,9]
说明：
输出结果中每个元素出现的次数，应与元素在两个数组中出现的次数一致。
我们可以不考虑输出结果的顺序。
进阶:
如果给定的数组已经排好序呢？你将如何优化你的算法？
如果 nums1 的大小比 nums2 小很多，哪种方法更优？
如果 nums2 的元素存储在磁盘上，磁盘内存是有限的，并且你不能一次加载所有的元素到内存中，你该怎么办？

[链接：https://leetcode-cn.com/problems/intersection-of-two-arrays-ii](https://leetcode-cn.com/problems/intersection-of-two-arrays-ii)
### 解
用的是c++ set_intersection的方法，两个数组排好序，两个指针分别从头开始遍历。
#### python code[52ms 11.8MB]
```
class Solution(object):
    def intersect(self, nums1, nums2):
        """
        :type nums1: List[int]
        :type nums2: List[int]
        :rtype: List[int]
        """
        point1 = 0
        point2 = 0
        nums1.sort()
        nums2.sort()
        res = []
        while point1<len(nums1) and point2<len(nums2):
            if nums1[point1] == nums2[point2]:
                res.append(nums2[point2])
                point2 += 1
                point1 += 1
            elif nums1[point1] < nums2[point2]:
                point1 += 1
            else:
                point2 += 1
        return res
```
#### c++ code[12ms 9MB]
发现用指针要快很多唉，不用的话是20ms。
```
class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        vector<int>::iterator it1 = nums1.begin();
        vector<int>::iterator it2= nums2.begin();
        sort(nums1.begin(),nums1.end());
        sort(nums2.begin(),nums2.end());
        while (it1!=nums1.end() && it2!=nums2.end())
        {
            if(*it1 == *it2){
                res.push_back(*it1);
                ++it1;
                ++it2;
            }
            else if(*it1 < *it2){
                ++it1;
            }
            else
            {
                ++it2;
            }
        }
        return res;
    }
};
```
后面是用的手写hash表，调整N的大小，发现到1e3的时候还不会报错，然后也没管open和close hash，也没报错......
#### c++ code[8ms 8.8MB]
```
const int N = 1e3;
class Hash{
    public:
        Hash(){
            memset(hash, 0, sizeof(hash));
            memset(count, 0, sizeof(count));
        }
        void Set(int n){
            int p = (n%N+N)%N;
            if(hash[p]){
                count[p] += 1;
            }
            else
            {
                hash[p]=true;
                count[p]=1;
            }
        }
        bool Get(int n){
            int p = (n%N+N)%N;
            return hash[p];
        }
        bool ReSet(int n){
            int p = (n%N+N)%N;
            bool flag = false;
            if (hash[p])
            {
                flag = true;
                --count[p];
                if(count[p]==0){
                    hash[p]=false;
                }
            }
            return flag;
        }
    private:
        bool hash[N];
        int count[N];
};

class Solution {
public:
    vector<int> intersect(vector<int>& nums1, vector<int>& nums2) {
        Hash saved;
        for(auto num:nums1){
            saved.Set(num);
        }
        vector<int> ret;
        for (auto num:nums2)
        {
            if (saved.ReSet(num))
            {
                ret.push_back(num);
            }
            
        }
        return ret;
    }
};
```
用c++的unordered_map和python的dict也是同样的思路,`Hash saved`改成`unordered_map<int,int> saved`，只不过用python的时候要注意`for key in dict`是用hash查找的，`for key in dict.keys()`不是。
