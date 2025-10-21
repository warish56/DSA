

# Prefix Sum

**Problem Link:** [LeetCode - Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/description/)

## Similar Problems

1. **Problem Link:** [Binary Subarrays With Sum](https://leetcode.com/problems/binary-subarrays-with-sum/description/?utm_source=chatgpt.com)



## Description

This problem teaches us the **prefix sum** technique, which is particularly useful when dealing with arrays containing negative numbers.

### Key Concepts:

1. **Prefix Sum Logic**: We keep track of the cumulative sum up to every character
2. **Mathematical Relationship**: 
   ```
   prefixSum[j] - prefixSum[i] = k
   ```
3. **Problem Goal**: Find if there exists any subarray whose sum equals `k`
4. **Why Not Sliding Window**: Traditional sliding window doesn't work with negative numbers
5. **Solution Strategy**: Since `prefixSum[j] - prefixSum[i] = k`, we search for `prefixSum[i] = prefixSum[j] - k`

## Solution

### Approach
- Use HashMap to store prefix sums and their frequencies
- For each element, calculate cumulative sum
- Check if `(current_sum - k)` exists in the map
- If found, add its frequency to the count
- Update the map with current sum

### Code

```java

public int subarraySum(int[] nums, int k) {
    HashMap<Integer, Integer> prefixMap = new HashMap<Integer, Integer>();
    
    int sum = 0;
    int count = 0;
    prefixMap.put(0, 1); // adding the 0 sum count as 1 in the intial state
    
    for(int num: nums){
        sum += num;
        
        // Check if we've seen (sum - k) before
        if(prefixMap.containsKey(sum - k)){
            count += prefixMap.get(sum - k);
        }
        
        // Update the frequency of current sum
        prefixMap.put(sum, prefixMap.getOrDefault(sum, 0) + 1);
    }
    
    return count;
}
```

### Explanation

1. **Initialization**: Create a HashMap to store prefix sums and their frequencies
2. **Iteration**: For each element, calculate the cumulative sum
3. **Check**: Look for `(current_sum - k)` in the map
   - If found, it means there's a subarray ending at current position with sum `k`
   - Add the frequency to count (handles multiple occurrences)
4. **Update**: Store current sum in the map with its frequency
5. **Result**: Return total count of subarrays with sum `k`

### Time Complexity: O(n)
### Space Complexity: O(n)