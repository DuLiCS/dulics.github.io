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

### 1528. Shuffle String


Given a string s and an integer array indices of the same length.

The string s will be shuffled such that the character at the ith position moves to indices[i] in the shuffled string.

Return the shuffled string.

 

Example 1:

```
Input: s = "codeleet", indices = [4,5,6,7,0,2,1,3]
Output: "leetcode"
Explanation: As shown, "codeleet" becomes "leetcode" after shuffling.
```

Example 2:
```
Input: s = "abc", indices = [0,1,2]
Output: "abc"
Explanation: After shuffling, each character remains in its position.
```


Example 3:

```
Input: s = "aiohn", indices = [3,1,4,2,0]
Output: "nihao"
```

Example 4:

```
Input: s = "aaiougrt", indices = [4,0,2,6,7,3,1,5]
Output: "arigatou"
```

Example 5:

```
Input: s = "art", indices = [1,0,2]
Output: "rat"
```

Constraints:

s.length == indices.length == n
1 <= n <= 100
s contains only lower-case English letters.
0 <= indices[i] < n
All values of indices are unique (i.e. indices is a permutation of the integers from 0 to n - 1).


#### Python3

```python

class Solution:
    def restoreString(self, s: str, indices: List[int]) -> str:
        result = ''
        for i in range(len(s)):
            position = indices.index(i)
            result = result + s[position]
            
            
        return result

```