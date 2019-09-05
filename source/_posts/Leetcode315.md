---
title: Leetcode315
tags: code
comment: true
date: 2019-09-05 12:09:00
---
### 题目描述
给定一个整数数组 nums，按要求返回一个新数组 counts。数组 counts 有该性质： counts[i] 的值是  nums[i] 右侧小于 nums[i] 的元素的数量。
示例:
输入: [5,2,6,1]
输出: [2,1,1,0] 
解释:
5 的右侧有 2 个更小的元素 (2 和 1).
2 的右侧仅有 1 个更小的元素 (1).
6 的右侧有 1 个更小的元素 (1).
1 的右侧有 0 个更小的元素.
[链接：https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self](https://leetcode-cn.com/problems/count-of-smaller-numbers-after-self)
### 解
一种做法为双重循环，但是python会tle，另一种做法就是想到了昨天的Leetcode300,维护一个返回的排序链表，这个也一样啦，就倒着遍历，然后将遍历过的元素维护一个排序list，用二分查找返回它插入时候的下标。因为算的只是小于，不是小于等于，所以还应该是bisect_left。喔，不过这种做法的时间复杂度仍然是N^2不是N*log(N),因为插入的时候后面的元素都要往后移，复杂度是O(N).
然后，vector初始重复元素的方法`vector<int> a(length,0)`,vector初始化二维数组的方法`vector<int> a(length,vector)`.
#### python code [176ms 15MB]
```
import bisect
class Solution(object):
    def countSmaller(self, nums):
        """
        :type nums: List[int]
        :rtype: List[int]
        """
        if nums == []:
            return []
        length = len(nums)
        counts = [0] * length
        tmp=[]
        tmp.append(nums[-1])
        counts[-1]=0
        for i in range(length-2, -1, -1):
            index = bisect.bisect_left(tmp, nums[i], 0, len(tmp))
            tmp.insert(index, nums[i])
            counts[i] = index
        return counts
```
#### c++ code [468ms 10MB]
```
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {
        vector<int> tmp;
        int length = nums.size();
        vector<int> counts(length, 0);
        if(length == 0){
            return counts;
        }
        for(int i=length-1;i>=0;i--){
            int index = lower_bound(tmp.begin(),tmp.end(), nums[i]) - tmp.begin();
            counts[i] = index;
            tmp.insert(tmp.begin()+index, nums[i]);
        }
        return counts;
    }
};
```
###  Not solved
爱豆说还有O(N*log(N))的做法，我还不知道哝。然后还有vector一堆初始化方法，我也不知道=_=.
