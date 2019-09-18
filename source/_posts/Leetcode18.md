---
title: Leetcode18:4Sum
tags: code
comment: true
date: 2019-09-01 12:09:00
---
### 题目描述：
给定一个包含 n 个整数的数组 nums 和一个目标值 target，判断 nums 中是否存在四个元素 a，b，c 和 d ，使得 a + b + c + d 的值与 target 相等？找出所有满足条件且不重复的四元组。

注意：

答案中不可以包含重复的四元组。

示例：

给定数组 nums = [1, 0, -1, 0, -2, 2]，和 target = 0。

满足要求的四元组集合为：
[
  [-1,  0, 0, 1],
  [-2, -1, 1, 2],
  [-2,  0, 0, 2]
]


[链接：https://leetcode-cn.com/problems/4sum](https://leetcode-cn.com/problems/4sum)

### 解

可以固定一个数，然后转换成Leetcode15找三个数的target的类似问题，之前有看Leetcode16的解(我的小旗没有倒！)，看到别人有用双指针，然后这次的思路就是先固定一个数，然后中间再固定一个数，用双指针去找另外两个数。

#### python code[1100ms 11.6MB]:

```
class Solution(object):
    def fourSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[List[int]]
        """
        size = len(nums)
        nums.sort()
        result = []
        for i in range(size-3):
            if i > 0 and nums[i]==nums[i-1]:
                continue
            little_target = target - nums[i]
            sub_result=[]
            sub_result = self.threeSum(nums[i+1:],little_target)
            
            for j in sub_result:
                j.append(nums[i])
                result.append(j)
        return result
               
    
    def threeSum(self, nums, target):
        result = []
        size = len(nums)
        if size == 3:
            r = nums[0]+nums[1]+nums[2]
            if r == target:
                result.append([nums[0],nums[1],nums[2]])
            return result
        
        for i in range(size - 2):
            if i>0 and nums[i]==nums[i-1]:
                continue
            left = i + 1
            right = size - 1
            while left<right:
                mid_result = nums[i]+nums[left]+nums[right]
                if left > i + 1 and nums[left] == nums[left-1]:
                    left = left + 1
                    continue
                if right < size - 1 and nums[right] == nums[right+1]:
                    right = right - 1
                    continue
                if mid_result == target:
                    result.append([nums[i],nums[left],nums[right]])
                    left = left + 1
                    right = right - 1
                elif mid_result < target:
                    left = left + 1
                else :
                    right = right - 1

        return result
```

#### 72ms version from leetcode
```
class Solution(object):
    def fourSum(self, nums, target):
        def findNsum(l, r, target, N, result, results):
            if r - l + 1 < N or N < 2 or target < nums[l] * N or target > nums[r] * N:
                return
            # two pointers solve sorted 2-sum problem
            if N == 2:
                while l < r:
                    s = nums[l] + nums[r]
                    if s == target:
                        results.append(result + [nums[l], nums[r]])
                        l += 1
                        while l < r and nums[l] == nums[l - 1]:
                            l += 1
                    elif s < target:
                        l += 1
                    else:
                        r -= 1
            else:
                for i in range(l, r + 1):
                    if i == l or (i > l and nums[i - 1] != nums[i]):
                        findNsum(i + 1, r, target - nums[i], N - 1, result + [nums[i]], results)
        nums.sort()
        results = []
        findNsum(0, len(nums) - 1, target, 4, [], results)
        return results
```

看上去思路好像跟我的差不多的样子，但是耗时小了很多，因为在调用threeNum的时候传了数组的slice进去，slice操作每次都要复制，耗时间和内存。
#### c++ code[48ms 9.1MB]:
```
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> result;
        sort(nums.begin(),nums.end());
        int length = nums.size();
        for(int i=0;i<length-3;i++){
            if(i>0 && nums[i-1] == nums[i])
                continue;
            for(int j=i+1;j<length-2;j++){
                if(j>i+1 && nums[j]==nums[j-1])
                    continue;
                int left = j+1;
                int right = length-1;
                int sum = target - nums[i] - nums[j];
                while(left<right){
                    if(nums[left]+nums[right] == sum){
                        vector<int> mid = {nums[i],nums[j], nums[left],nums[right]};
                        result.push_back(mid);
                        while(left<right && nums[left+1]==nums[left])
                            left++;
                        while(left<right && nums[right-1]==nums[right])
                            right--;
                        left++;
                        right--;
                    }
                    else if(nums[left]+nums[right] < sum){
                        left++;
                    }
                    else{
                        right--;
                    }
                }
            }
        }
        return result;
    }
};
```
嗯一开始想按照1100ms python的版本来翻译c++，翻译完觉得有点丑，虽然我没有强迫症，但是我是个要脸的人，所以就用循环了。

## 爱豆指针引用小讲堂

### C++
`初始内存：memory = [0,0,0,0,0,0,0,0,0,0]`

step 1:
`int a = 10; // memory[2]=10`
`memory = [0, 0, 10, 0, 0, 0, 0, 0, 0, 0]`</br>

step2:
`int *b = &a; // memory[3]=2 a的地址是2`
`memory = [0, 0, 10, 2, 0, 0, 0, 0, 0, 0]`</br>

step3:
`int &c = a; // 等价于 int *d = &a; memory[4]=2`
`memory = [0, 0, 10, 2, 2, 0, 0, 0, 0, 0]`</br>

step4:
`c=20; //memory[memory[4]]=20`
`memory = [0, 0, 20, 2, 2, 0, 0, 0, 0, 0]`</br>

step5:
`c=30;`
`memory = [0, 0, 30, 2, 2, 0, 0, 0, 0, 0]`</br>

pointer is a variable which stores addr, reference is better-looking pointer.
### python
python中都是引用
```
func(a: list):   
     # 1    
     a[0] = 10    
     a.sort()        # 外部传进来的list会被排序
     
     # 2    
     b = sorted(a)   # 外部传进来的list不会被排序，sorted生成一个新list  
     
     # 3    
     a = sorted(a)   # 外部传进来的list不会被排序，函数内新建了一个变量，见下一个block
     a_2 = sorted(a_1)      
     
     # 4    
     a[:] = sorted(a)    # 外部传进来的list会被排序，相当于sorted后的list又赋值给a本身
     b = sorted(a)    
     for i in range(len(a)):        
        a[i] = b[i]
```
辅助说明3为什么不会被排序
```
def func(a):
    a = 3

a = 2
func(a)

Out: a=2
```
相当于在函数内新建了一个变量，啊把引用看成指针好了，相当于在函数内新建了个指针。

## 爱豆python slice小讲堂
#### 1. slice
```
[In]: nums = list(range(5))

[In]: nums[:2][0] = 7

[OUT]: nums: [0,1,2,3,4]
```
相当于拷贝了一份新list
#### 2. slice assignment
```
[In]: nums = list(range(5))

[In]: nums[:2]=(4,5)

[OUT]; nums: [4,5,2,3,4]
```
相当于在原来的list上进行赋值
