- whenever you see an array having range of elements [1,n] -- Apply cycle sort 
- where the correct index value is element - 1; for [1,n]
  and correct index value is element ; for [0,n] 
## constraints 
When solving LeetCode problems, understanding the constraints is key to choosing the right algorithm and data structure. The constraints you provided are quite common and relate to the size of the input and the maximum value of variables. Here's a breakdown of what they mean and how to use them to your advantage.

The constraints you listed are:

- `1 <= banned.length <= 10^4`
    
- `1 <= banned[i], n <= 10^4`
    
- `1 <= maxSum <= 10^9`
    

Let's break them down.

- **`1 <= banned.length <= 10^4`**: This constraint tells you the size of the `banned` array. A length of up to 104 suggests that an **O(N2)** algorithm, where N is the length of the array, might be too slow. A typical computer can perform around 108 operations per second. An O(N2) solution with N=104 would require (104)2=108 operations, which is on the edge of what's acceptable. An O(NlogN) or O(N) solution is likely what the problem expects. 🧠
    
    - **O(N2)** algorithms include nested loops that iterate through the array.
        
    - **O(NlogN)** algorithms often involve sorting.
        
    - **O(N)** algorithms usually involve a single pass through the data.
        
- **`1 <= banned[i], n <= 10^4`**: This constraint defines the **range of values** inside the `banned` array and for a variable `n`. A maximum value of 104 is relatively small. This could indicate that you can use an array or a **hash set** to keep track of the banned numbers, as the values themselves are within a manageable range. For example, you could create a boolean array `isBanned` of size 104+1 and mark the banned numbers.
    
- **`1 <= maxSum <= 10^9`**: This constraint specifies the maximum value of a variable `maxSum`. A value of 109 is quite large. This immediately suggests that a **brute-force approach** that iterates up to `maxSum` is not feasible, as it would be too slow. This large value often points to a solution that uses a **binary search** or a mathematical formula. Binary search is efficient for finding a value in a large range, often reducing the search space by half in each step. The time complexity for a binary search on a range up to 109 would be O(log109), which is approximately O(30), a very fast operation.
    

---

[Common Tricks and Strategies]
- **Analyze the scale**:
    
    - **Small `N` (e.g., N≤50)**: An **exponential time complexity** (e.g., O(2N), O(N!)) might be acceptable. This is often seen in problems requiring exhaustive search or backtracking.
        
    - **Medium `N` (e.g., N≤103)**: An **O(N2)** solution is likely fine.
        
    - **Large `N` (e.g., N≤105)**: Look for **O(NlogN)** or **O(N)** solutions.
        
    - **Extremely Large `N` (e.g., N>106)**: An **O(N)** or even **O(logN)** solution is required.
        
- **Look for patterns in the constraints**:
    
    - **Large value, small count**: If you have a large range of values but only a few of them are "special" (e.g., banned), you can use a **hash set** or a **hash map** to store those special values. This gives you O(1) average time complexity for lookups.
        
    - **Large maximum value**: As mentioned, a large maximum value like 109 almost always suggests a non-linear approach like **binary search**, **dynamic programming**, or a greedy algorithm.
        
    - **Small maximum value**: If the values themselves are small (e.g., up to 105), you can use an **array for frequency counting** or a boolean array for marking presence.
        
- **Match constraints to algorithms**:
    
    - **Sorting**: If the problem involves finding relationships between elements, consider sorting the input first. This transforms the problem from an O(N2) to an O(NlogN) search.
        
    - **Prefix sums**: If you need to calculate sums of sub-arrays repeatedly, use a **prefix sum array** to get the sum of any sub-array in O(1) time.
        
    - **Two pointers**: This technique works well on sorted arrays and can reduce the complexity from O(N2) to O(N).