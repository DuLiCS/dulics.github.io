---

layout: post
title:Leetcode Rush 
subtitle: May 07
tags: [Algorithm, Leetcode]

---

### 1431. Kids With the Greatest Number of Candies


Given the array candies and the integer extraCandies, where candies[i] represents the number of candies that the ith kid has.

For each kid check if there is a way to distribute extraCandies among the kids such that he or she can have the greatest number of candies among them. Notice that multiple kids can have the greatest number of candies.

 

Example 1:

```
Input: candies = [2,3,5,1,3], extraCandies = 3
Output: [true,true,true,false,true] 
Explanation: 
Kid 1 has 2 candies and if he or she receives all extra candies (3) will have 5 candies --- the greatest number of candies among the kids. 
Kid 2 has 3 candies and if he or she receives at least 2 extra candies will have the greatest number of candies among the kids. 
Kid 3 has 5 candies and this is already the greatest number of candies among the kids. 
Kid 4 has 1 candy and even if he or she receives all extra candies will only have 4 candies. 
Kid 5 has 3 candies and if he or she receives at least 2 extra candies will have the greatest number of candies among the kids. 
```

Example 2:

```
Input: candies = [4,2,1,1,2], extraCandies = 1
Output: [true,false,false,false,false] 
Explanation: There is only 1 extra candy, therefore only kid 1 will have the greatest number of candies among the kids regardless of who takes the extra candy.

```
Example 3:

```
Input: candies = [12,1,12], extraCandies = 10
Output: [true,false,true]
 
```
```
Constraints:

2 <= candies.length <= 100
1 <= candies[i] <= 100
1 <= extraCandies <= 50
```


### Python3:

```
class Solution:
    def kidsWithCandies(self, candies: List[int], extraCandies: int) -> List[bool]:
        result = []
        for i in range(len(candies)):
            temp = candies[:]
            temp[i] = temp[i] + extraCandies
            result.append((max(temp)==temp[i]))
        
        return result

```


### 1342. Number of Steps to Reduce a Number to Zero


Given a non-negative integer num, return the number of steps to reduce it to zero. If the current number is even, you have to divide it by 2, otherwise, you have to subtract 1 from it.

 

Example 1:

```
Input: num = 14
Output: 6
Explanation: 
Step 1) 14 is even; divide by 2 and obtain 7. 
Step 2) 7 is odd; subtract 1 and obtain 6.
Step 3) 6 is even; divide by 2 and obtain 3. 
Step 4) 3 is odd; subtract 1 and obtain 2. 
Step 5) 2 is even; divide by 2 and obtain 1. 
Step 6) 1 is odd; subtract 1 and obtain 0.
```

Example 2:

```
Input: num = 8
Output: 4
Explanation: 
Step 1) 8 is even; divide by 2 and obtain 4. 
Step 2) 4 is even; divide by 2 and obtain 2. 
Step 3) 2 is even; divide by 2 and obtain 1. 
Step 4) 1 is odd; subtract 1 and obtain 0.
```
Example 3:

```
Input: num = 123
Output: 12
``` 

Constraints:

```
0 <= num <= 10^6
```
### C:

```c

int numberOfSteps (int num){
    int flag = 0;
        while (num != 0){
            if (num % 2 == 0){
                num /= 2;
                flag += 1;}
            else{
                num -= 1;
                flag += 1;
            }
        
       

}
     return flag;}
```

### Python3

```python
class Solution:
    def numberOfSteps (self, num: int) -> int:
        flag = 0
        while (num != 0):
            if num % 2 == 0:
                num /= 2
                flag += 1
            else:
                num -= 1
                flag += 1
        
        return flag
```


























