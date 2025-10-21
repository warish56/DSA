

# Kadane's Algorithm

**Problem Link:** [LeetCode - Maximum Subarray](https://leetcode.com/problems/maximum-subarray/description/)

## Similar Problems

1. **Problem Link:** [Maximum Product Subarray](https://leetcode.com/problems/maximum-product-subarray/description/)
2. **Problem Link:** [Maximum Sum Circular Subarray](https://leetcode.com/problems/maximum-sum-circular-subarray/description/)
3. **Problem Link:** [Best Time to Buy and Sell Stock](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/description/)



## Description

This problem teaches us **Kadane's Algorithm**, a dynamic programming technique to find the maximum sum of a contiguous subarray efficiently.

### Key Concepts:

1. **Local vs Global Maximum**: Track both the maximum sum ending at current position and the overall maximum
2. **Decision at Each Step**: 
   ```
   At each index, decide: Should I extend the existing subarray or start fresh?
   ```
3. **Problem Goal**: Find the contiguous subarray with the largest sum
4. **Why This Works**: At each position, we only need to know if adding current element to previous sum is better than starting new
5. **Solution Strategy**: Keep track of `currentSum` and `maxSum`, update them as we traverse the array

## Solution

### Approach
- Initialize `currentSum` and `maxSum` with the first element
- Traverse the array from the second element
- At each position, decide: `max(current element, currentSum + current element)`
- Update `maxSum` if `currentSum` is greater
- Return the maximum sum found

### Code


```java
public int maxSubArray(int[] nums) {
    int currentSum = 0;
    int maxSum = Integer.MIN_VALUE;
    
    for(int num : nums) {
        currentSum += num;
        maxSum = Math.max(maxSum, currentSum);
        
        // If currentSum becomes negative, reset it to 0
        if(currentSum < 0) {
            currentSum = 0;
        }
    }
    
    return maxSum;
}
```

### Explanation

1. **Initialization**: Start with the first element as both current and maximum sum
2. **Iteration**: For each element starting from index 1:
   - **Decision Point**: Compare adding current element to existing sum vs starting fresh
   - If `currentSum + nums[i]` is less than `nums[i]` alone, it's better to start a new subarray
   - This is equivalent to: if `currentSum < 0`, reset to 0 in the alternative approach
3. **Update Maximum**: After each decision, check if we've found a new maximum
4. **Key Insight**: A negative sum will only decrease future sums, so we discard it
5. **Result**: Return the maximum sum found during traversal

### Example Walkthrough

**Input:** `[-2, 1, -3, 4, -1, 2, 1, -5, 4]`

| Index | Value | currentSum | maxSum | Explanation |
|-------|-------|------------|--------|-------------|
| 0     | -2    | -2         | -2     | Initialize |
| 1     | 1     | 1          | 1      | 1 > (-2+1), start fresh |
| 2     | -3    | -2         | 1      | (1-3) = -2 |
| 3     | 4     | 4          | 4      | 4 > (-2+4), start fresh |
| 4     | -1    | 3          | 4      | (4-1) = 3 |
| 5     | 2     | 5          | 5      | (3+2) = 5 |
| 6     | 1     | 6          | 6      | (5+1) = 6 |
| 7     | -5    | 1          | 6      | (6-5) = 1 |
| 8     | 4     | 5          | 6      | (1+4) = 5 |

**Output:** `6` (subarray `[4, -1, 2, 1]`)

### Time Complexity: O(n)
### Space Complexity: O(1)