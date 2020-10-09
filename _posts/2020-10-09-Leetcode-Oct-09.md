---

layout: post
title: Leetcode Rush
subtitle: Oct 09
tags: [Algorithm, Leetcode]

---


### 1512. Number of Good Pairs

Given an array of integers nums.

A pair (i,j) is called good if nums[i] == nums[j] and i < j.

Return the number of good pairs.

 

Example 1:
```
Input: nums = [1,2,3,1,1,3]
Output: 4
Explanation: There are 4 good pairs (0,3), (0,4), (3,4), (2,5) 0-indexed.
```

Example 2:

```
Input: nums = [1,1,1,1]
Output: 6
Explanation: Each pair in the array are good.
```

Example 3:

```
Input: nums = [1,2,3]
Output: 0
```

Constraints:

```
1 <= nums.length <= 100
1 <= nums[i] <= 100
```

#### Python3

```python

class Solution:
    def numIdenticalPairs(self, nums: List[int]) -> int:
        flag = 0
        for i in range(len(nums)):
            temp = nums[i]
            for j in range(len(nums)-i-1):
                if temp == nums[i+j+1]:
                    flag = flag + 1
            
        return flag
```