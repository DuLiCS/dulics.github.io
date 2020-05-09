---

layout: post
title: Leetcode Rush
subtitle: May 09
tags: [Algorithm, Leetcode]

---


### 1313. Decompress Run-Length Encoded List


We are given a list nums of integers representing a list compressed with run-length encoding.

Consider each adjacent pair of elements [freq, val] = [nums[2*i], nums[2*i+1]] (with i >= 0).  For each such pair, there are freq elements with value val concatenated in a sublist. Concatenate all the sublists from left to right to generate the decompressed list.

Return the decompressed list.

 

Example 1:

```
Input: nums = [1,2,3,4]
Output: [2,4,4,4]
Explanation: The first pair [1,2] means we have freq = 1 and val = 2 so we generate the array [2].
The second pair [3,4] means we have freq = 3 and val = 4 so we generate [4,4,4].
At the end the concatenation [2] + [4,4,4] is [2,4,4,4].
```

Example 2:

```
Input: nums = [1,1,2,3]
Output: [1,3,3]
```

Constraints:

```
2 <= nums.length <= 100
nums.length % 2 == 0
1 <= nums[i] <= 100
```

### python3:

```python
class Solution:
    def decompressRLElist(self, nums: List[int]) -> List[int]:
        result = []
        for i in range(len(nums)//2):
            for j in range(nums[2*i]):
                result.append(nums[2*i+1])
                
        return result
```



### 1295. Find Numbers with Even Number of Digits

Given an array nums of integers, return how many of them contain an even number of digits.
 

Example 1:

```
Input: nums = [12,345,2,6,7896]
Output: 2
Explanation: 
12 contains 2 digits (even number of digits). 
345 contains 3 digits (odd number of digits). 
2 contains 1 digit (odd number of digits). 
6 contains 1 digit (odd number of digits). 
7896 contains 4 digits (even number of digits). 
Therefore only 12 and 7896 contain an even number of digits.
```


Example 2:

```
Input: nums = [555,901,482,1771]
Output: 1 
Explanation: 
Only 1771 contains an even number of digits.
``` 

Constraints:

```
1 <= nums.length <= 500
1 <= nums[i] <= 10^5
```

### python3:

```python3

class Solution:
    def findNumbers(self, nums: List[int]) -> int:
        flag = 0
        for num in nums:
            tag = 0
            while(num>0):
                num = num//10
                tag += 1

            if tag%2 == 0:
                flag += 1

        return flag
 ```
