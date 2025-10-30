#Questions- #leetcode 442[medium] , 41 [hard]
- If the number range is btw [1,n]
  then index will be = value- 1;
- if the number range is btw [0,n]
    then index will be = value;
    as index starts from 0 and value start from 0 and 1;

Question-41
Given an unsorted integer array `nums`. Return the _smallest positive integer_ that is _not present_ in `nums`.
You must implement an algorithm that runs in `O(n)` time and uses `O(1)` auxiliary space.
explanation -- what we have to do is to apply cycle sort but how??
see first you have to see the example you have to find the smallest positive integer that is missing 

**Input:** nums = [1,2,0]
**Output:** 3
**Explanation:** The numbers in the range [1,2] are all in the array.


**Input:** nums = [3,4,-1,1]
**Output:** 2
**Explanation:** 1 is in the array but 2 is missing.