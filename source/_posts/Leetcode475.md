---
title: Leetcode474
tags: code
comment: true
date: 2019-09-12 11:11:00
---
### 题目描述：
冬季已经来临。 你的任务是设计一个有固定加热半径的供暖器向所有房屋供暖。
现在，给出位于一条水平线上的房屋和供暖器的位置，找到可以覆盖所有房屋的最小加热半径。
所以，你的输入将会是房屋和供暖器的位置。你将输出供暖器的最小加热半径。
说明:
给出的房屋和供暖器的数目是非负数且不会超过 25000。
给出的房屋和供暖器的位置均是非负数且不会超过10^9。
只要房屋位于供暖器的半径内(包括在边缘上)，它就可以得到供暖。
所有供暖器都遵循你的半径标准，加热的半径也一样。
示例 1:
输入: [1,2,3],[2]
输出: 1
解释: 仅在位置2上有一个供暖器。如果我们将加热半径设为1，那么所有房屋就都能得到供暖。
示例 2:
输入: [1,2,3,4],[1,4]
输出: 1
解释: 在位置1, 4上有两个供暖器。我们需要将加热半径设为1，这样所有房屋就都能得到供暖。

[链接：https://leetcode-cn.com/problems/heaters](https://leetcode-cn.com/problems/heaters)
### 解：
通过二分法找出房屋距离供暖器的最大距离。
#### python code[444ms 14.4MB]
用的`<=`的二分
```
class Solution(object):
    def get_closest(self, target, heaters, left, right):
        while left <= right:
            mid = (left + right) //2
            if heaters[mid]<=target:
                left = mid + 1
            else:
                right = mid - 1
        return left
            
    def findRadius(self, houses, heaters):
        """
        :type houses: List[int]
        :type heaters: List[int]
        :rtype: int
        """
        heaters.sort()
        length = len(heaters)
        max_dis = 0
        for house in houses:
            index = self.get_closest(house, heaters, 0, length-1)
            if index>=length:
                dis = house - heaters[-1]
            elif index==0:
                dis = heaters[0] - house
            else:
                dis = min(heaters[index]-house,house-heaters[index-1])
            if max_dis<dis:
                max_dis = dis
        return max_dis
```
#### python code[324ms 14.4MB]
用了python自带的二分bisect_left
```
class Solution(object):
    def findRadius(self, houses, heaters):
        """
        :type houses: List[int]
        :type heaters: List[int]
        :rtype: int
        """
        heaters.sort()
        length = len(heaters)
        max_dis = 0
        for house in houses:
            
            index = bisect.bisect_left(heaters, house, 0, length)
            if index>=length:
                dis = house - heaters[-1]
            elif index==0:
                dis = heaters[0] - house
            else:
                dis = min(heaters[index]-house,house-heaters[index-1])
            if max_dis<dis:
                max_dis = dis
        return max_dis
```
#### c++ code[88ms 11MB]
用了`<`的二分
```
class Solution {
public:
    int get_closest(vector<int>& heaters, int target, int left, int right){
        while (left<right)
        {
            int mid = (left+right)/2;
            if(heaters[mid]<=target)
                left = mid + 1;
            else
                right = mid;
        }
        return left;
    }
    int findRadius(vector<int>& houses, vector<int>& heaters) {
        int max_dis = 0;
        sort(heaters.begin(),heaters.end());
        for (auto& house:houses)
        {
            int dis = 0;
            int index = get_closest(heaters, house, 0, heaters.size());
            if(index>=heaters.size())
                dis = house - heaters[heaters.size()-1];
            else if(index<=0)
                dis = heaters[0] - house;
            else
                dis = min(heaters[index]-house,house-heaters[index-1]);
            if(max_dis<dis)
                max_dis = dis;
        }
        return max_dis;
    }
};
```
#### c++ code[88ms 11.2MB]
用了c++自带的二分lower_bound
```
class Solution {
public:
    int findRadius(vector<int>& houses, vector<int>& heaters) {
        int max_dis = 0;
        sort(heaters.begin(),heaters.end());
        for (auto& house:houses)
        {
            int dis = 0;
            int index = lower_bound(heaters.begin(),heaters.end(),house)-heaters.begin();
            if(index>=heaters.size())
                dis = house - heaters[heaters.size()-1];
            else if(index<=0)
                dis = heaters[0] - house;
            else
                dis = min(heaters[index]-house,house-heaters[index-1]);
            if(max_dis<dis)
                max_dis = dis;
        }
        return max_dis;
    }
};
```
(嘻，其实觉得题目出的并不是很规范，应该是供暖器与锅炉房的距离吧，哈哈哈哈，来自北方人的骄傲:D
