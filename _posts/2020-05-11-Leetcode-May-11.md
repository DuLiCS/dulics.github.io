---

layout: post
title: Leetcode Rush
subtitle: May 11
tags: [Algorithm, Leetcode]

---


### 1389. Create Target Array in the Given Order

Given two arrays of integers nums and index. Your task is to create target array under the following rules:

Initially target array is empty.
From left to right read nums[i] and index[i], insert at index index[i] the value nums[i] in target array.
Repeat the previous step until there are no elements to read in nums and index.
Return the target array.

It is guaranteed that the insertion operations will be valid.

 

Example 1:

```
Input: nums = [0,1,2,3,4], index = [0,1,2,2,1]
Output: [0,4,1,3,2]
Explanation:
nums       index     target
0            0        [0]
1            1        [0,1]
2            2        [0,1,2]
3            2        [0,1,3,2]
4            1        [0,4,1,3,2]
```

Example 2:

```
Input: nums = [1,2,3,4,0], index = [0,1,2,3,0]
Output: [0,1,2,3,4]
Explanation:
nums       index     target
1            0        [1]
2            1        [1,2]
3            2        [1,2,3]
4            3        [1,2,3,4]
0            0        [0,1,2,3,4]
```

Example 3:

```
Input: nums = [1], index = [0]
Output: [1]
```

Constraints:

```
1 <= nums.length, index.length <= 100
nums.length == index.length
0 <= nums[i] <= 100
0 <= index[i] <= i
```


### python3:
```python
class Solution:
    def createTargetArray(self, nums: List[int], index: List[int]) -> List[int]:
        result = []

        for i in range(len(nums)):
            result.insert(index[i],nums[i])
        
        return result
```

### 997. Find the Town Judge

In a town, there are N people labelled from 1 to N.  There is a rumor that one of these people is secretly the town judge.

If the town judge exists, then:

The town judge trusts nobody.
Everybody (except for the town judge) trusts the town judge.
There is exactly one person that satisfies properties 1 and 2.
You are given trust, an array of pairs trust[i] = [a, b] representing that the person labelled a trusts the person labelled b.

If the town judge exists and can be identified, return the label of the town judge.  Otherwise, return -1.

 

Example 1:

```
Input: N = 2, trust = [[1,2]]
Output: 2
```


Example 2:

```
Input: N = 3, trust = [[1,3],[2,3]]
Output: 3
```

Example 3:

```
Input: N = 3, trust = [[1,3],[2,3],[3,1]]
Output: -1
```

Example 4:


```
Input: N = 3, trust = [[1,2],[2,3]]
Output: -1
```

Example 5:

```
Input: N = 4, trust = [[1,3],[1,4],[2,3],[2,4],[4,3]]
Output: 3
```

Note:

1 <= N <= 1000
trust.length <= 10000
trust[i] are all different
trust[i][0] != trust[i][1]
1 <= trust[i][0], trust[i][1] <= N

### python3:

```python
class Solution:
    def findJudge(self, N: int, trust: List[List[int]]) -> int:
        people = [0]*N
        judge = [0]*N
        for i in trust:
            people[i[0]-1] += 1
            judge[i[1]-1] += 1

        for j in range(len(people)):
            if people[j] == 0:
                if judge[j] == N-1:
                    return j+1
                else:
                    return -1
            
        return -1
```