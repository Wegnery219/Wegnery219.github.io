---
title: Leetcode15+16
tags: code
comment: true
---
leetcode 15+16
## Leetcode 15

### 题目描述：
```
Given an array nums of n integers, are there elements a, b, c in nums such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.

Note:

The solution set must not contain duplicate triplets.

Example:

Given array nums = [-1, 0, 1, 2, -1, -4],

A solution set is:
[
  [-1, 0, 1],
  [-1, -1, 2]
]
```

### 解

因为和为0一定是正数加负数，所以可以将原始list按照正负为界限化成两部分。但是在找第三个数(0-nums[i]-nums[j])的时候可能存在两种情况，一种是第三个数也是前两个数中的一个(存在重复数的情况)，另一种是第三个数是一个新的数。用hash解决第一种情况，用一个hash表来统计每一个数字出现的次数，在hash表的key中寻找第三个数，如果第三个数出现在i和j中了，就判断第三个数的出现次数，如果没有，并且在hash key中，即第二种情况，需要考虑重复问题，即之前可能枚举过了这种情况。用一个约束`target<nums[i] or target>nums[j]`来解决重复。

#### python code[356ms 15.2MB]:

```
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        hash_d={}
        result = []
        for num in nums:
            hash_d[num]=hash_d.get(num, 0)+1
        
        negative = [i for i in hash_d if i < 0]
        positive = [i for i in hash_d if i >= 0]
        
        if hash_d.get(0)>=3:
          result.append([0,0,0])
        for i in negative:
            for j in positive:
                target = 0 - i - j
                if target in hash_d:
                    if target in (i, j) and hash_d[target]>=2:
                        result.append([target, i, j])
                    if target < i or target > j:
                        result.append([target, i, j])

        return result
```

#### c++ code[248ms 15.4MB]:
```
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<int> negtive;
        vector<int> positive;
        vector<vector<int>> result;
        unordered_map<int, int> dict;
        for (auto num: nums) {
            ++dict[num];
        }

        for (auto it: dict) {
            if (it.first < 0) {
                negtive.push_back(it.first);
            }
            if (it.first >= 0) {
                positive.push_back(it.first);
            }
        }
        if (dict.count(0) && dict[0] >= 3)
        {
            /* code */
            result.push_back({0,0,0});
        }
        
        for(int i=0; i<negtive.size(); i++){
            for(int j=0;j<positive.size();j++){
                int target = 0 - negtive[i] - positive[j];
                if(dict.find(target) != dict.end()){
                    if(target==negtive[i] || target == positive[j]){
                        if (dict[target] >= 2)
                        {
                            result.push_back({target, negtive[i], positive[j]});

                        }
                        
                    }
                    if(target<negtive[i] || target>positive[j]){
                        result.push_back({target, negtive[i], positive[j]});
                    }
                }
            }
        }
        return result;
    }
};
```

#### 我[idol](https://sf-zhou.github.io)写的代码:
```
#include <unordered_map>
#include <vector>

using std::unordered_map;
using std::vector;

class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        unordered_map<int, int> hash_d;
        vector<vector<int>> result;
        
        for (auto num: nums) {
            ++hash_d[num];
        }
        
        vector<int> negative;
        vector<int> positive;
        for (auto it: hash_d) {
            if (it.first < 0) {
                negative.push_back(it.first);
            }
            if (it.first >= 0) {
                positive.push_back(it.first);
            }
        }
        
        if (hash_d.count(0) && hash_d[0] >= 3) {
            result.push_back({0, 0, 0});
        }
        for (auto i: negative) {
            for (auto j: positive) {
                auto target = 0 - i - j;
                if (hash_d.count(target)) {
                    if ((target == i || target == j) && hash_d.count(target) && hash_d[target] >= 2) {
                        result.push_back({target, i, j});
                    }
                    if (target < i || target > j) {
                        result.push_back({target, i, j});
                    }
                }
            }
        }
        
        return result;
    }
};

```
啧，果然爱豆之所以能被称为爱豆。[打call打call打call！]

## Leetcode16

### 题目描述：
```
Given an array nums of n integers and an integer target, find three integers in nums such that the sum is closest to target. Return the sum of the three integers. You may assume that each input would have exactly one solution.

Example:

Given array nums = [-1, 2, 1, -4], and target = 1.

The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).
```

### 解

嗯看到这道题的时候比较暴躁[因为我刚被push]，所以我的想法就是三重循环O(N^3)[强行找理由]，然后就tle了哈哈哈哈嘻嘻嘻好开心:),后来在爱豆的循循善诱下，找第三个数的时候可以用二分查找找出比当前`target-nums[i]-nums[j]`大的最小的数的下标，因为使用二分查找都是排好序的，所以这个下标减去1就是比当前要找的数小的最大的数的下标，比较一下这两个数和i，j确定的数和target的大小关系，再和当前min比，时间复杂度可以降为O(N^2log(N)),之后第一次的python代码长酱样：
```
# class Solution(object):
#     def get_closest(begin, end, nums, target):
#         if begin<end:

def threeSumClosest(nums, target):
    """
    :type nums: List[int]
    :type target: int
    :rtype: int
    """
    nums.sort()
    min_num = 10000000
    result = []
    if len(nums)==3:
        return nums[0]+nums[1]+nums[2]
    for i in range(len(nums)):
        for j in range(i+1, len(nums)):
            goal = target - nums[i] - nums[j]
            begin = j + 1
            end = len(nums)
            while begin<end:
                mid = (begin+end)//2
                if nums[mid] < goal:
                    begin = mid+1
                else:
                    end = mid
            if begin<len(nums) and begin>j+1:
                cmp1 = abs(goal - nums[begin])
                cmp2 = abs(goal - nums[begin-1])
                if cmp1<cmp2 and cmp1<min_num:
                    min_num=cmp1
                    result=[nums[i],nums[j],nums[begin]]
                elif cmp2<cmp1 and cmp2<min_num:
                    min_num=cmp2
                    result=[nums[i],nums[j],nums[begin-1]]
            elif begin>=len(nums):
                tmp = abs(goal - nums[len(nums)-1])
                if tmp<min_num:
                    min_num=tmp
                    result = [nums[i], nums[j],nums[len(nums)-1]]
            elif begin<=j+1:
                tmp = abs(goal-nums[j+1])
                if tmp<min_num:
                    min_num=tmp
                    result = [nums[i],nums[j],nums[j+1]]
    return result


if __name__ == "__main__":
    print(threeSumClosest([-1,2,1,-4],1)) # result [-4,2,2] =?=
```
wa的情况是会出现重复数字，比如[2,-2,4,5,6]最终结果是[2,2,4],原因是如果存在begin=end=j+1=len(nums)的情况，`begin>=len(nums)`满足不代表begin-1可用。然后在人间天使，啊也就是我爱豆的指导下，最后的代码：

#### python code[1416ms 11.7MB]：
```
class Solution(object):
    def threeSumClosest(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: int
        """
        nums.sort()
        min_num = 1000000000
        result = []
        if len(nums)==3:
            return nums[0]+nums[1]+nums[2]
        for i in range(len(nums)):
            for j in range(i+1, len(nums)):
                goal = target - nums[i] - nums[j]
                begin = j + 1
                end = len(nums)
                while begin<end:
                    mid = (begin+end)//2
                    if nums[mid] < goal:
                        begin = mid+1
                    else:
                        end = mid
                if begin > j+1:
                    tmp = abs(goal - nums[begin - 1])
                    if tmp<min_num:
                        min_num=tmp
                        result = [nums[i], nums[j],nums[begin - 1]]
                if begin < len(nums):
                    tmp = abs(goal-nums[begin])
                    if tmp<min_num:
                        min_num=tmp
                        result = [nums[i],nums[j],nums[begin]]
        return sum(result)
```
简言之就是后面几种情况可以合并，begin>j+1时代表begin-1可用，begin < len(nums)时代表begin可用。但是时间空间看上去还是很吓人的样子，嗯我会看别人的思路的(小旗迎风飘)。
#### cpp code[68ms 8.7MB]
```
class Solution {
public:
    int threeSumClosest(vector<int>& nums, int target) {
        sort(nums.begin(),nums.end());
        int min_num = 0x7fffffff;
        int result=0;
        if(nums.size()==3){
            result = nums[0]+nums[1]+nums[2];
            return result;
        }
        for(int i=0; i<nums.size();i++){
            for (int j=i+1; j<nums.size();j++){
                int goal = target - nums[i] - nums[j];
                int begin = j+1;
                int end = nums.size();
                while (begin<end)
                {
                    /* code */
                    int mid = (begin+end)/2;
                    if (nums[mid]<goal)
                    {
                        begin = mid+1;
                    }
                    else
                    {
                        end = mid;
                    }
                    
                }
                if (begin<nums.size())
                {
                    int tmp = abs(goal - nums[begin]);
                    if(tmp<min_num){
                        min_num = tmp;
                        result = nums[i]+nums[j]+nums[begin];
                    }
                }
                if (begin>j+1){
                    int tmp = abs(goal - nums[begin-1]);
                    if(tmp<min_num){
                        min_num = tmp;
                        result = nums[i]+nums[j]+nums[begin-1];
                    }
                }
            }
        }
        return result;
    }
};
```

## 总结
1. 二分查找(复习查找算法)
2. 要记得去看leetcode16别人的算法
3. c++ unordered_map
4. 每次的边界情况要考虑清楚

最后我爱豆真的是个很好的人啊，感觉看他的blog可以学会很多东西，嗯文盲词穷式宣传的感觉...[打call！](https://sf-zhou.github.io)
