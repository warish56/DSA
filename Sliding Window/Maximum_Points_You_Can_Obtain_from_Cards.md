
# Maximum Points You Can Obtain from Cards

**Problem Link:** [LeetCode - Maximum Points You Can Obtain from Cards](https://leetcode.com/problems/maximum-points-you-can-obtain-from-cards/description/?utm_source=chatgpt.com)

## Description

This problem requires finding the maximum points you can obtain by choosing cards from either end at a time.

### Key Insight

While this looks like a difficult problem, we can solve it using a clever **reverse approach**:

- Instead of finding the maximum points directly, we find the **minimum subarray** using sliding window
- At the end, we return `totalPoints - minSubarray`

### Why This Works

- We can choose `k` cards from either end
- This means we're leaving `(n-k)` cards in the middle
- If we find the minimum sum of `(n-k)` consecutive cards, the remaining cards will give us the maximum points

## Solution

### Approach
- Calculate total sum of all cards
- Find minimum sum of `(n-k)` consecutive cards using sliding window
- Return `totalSum - minSubarray`

### Code

```java
public int maxScore(int[] cardPoints, int k) {
    int totalSum = 0;
    for(int point: cardPoints){
        totalSum += point;
    }

    // If k >= array length, we can take all cards
    if(k >= cardPoints.length){
        return totalSum;
    }

    int minWindow = cardPoints.length - k;
    int i = 0, j = 0;
    int min = Integer.MAX_VALUE;
    int sum = 0;

    while(j < cardPoints.length){
        sum += cardPoints[j];

        if(j - i + 1 < minWindow){
            j++;
        } else if(j - i + 1 == minWindow){
            min = Math.min(min, sum);
            sum -= cardPoints[i];
            i++;
            j++;
        }
    }

    return totalSum - min;
}
```

### Explanation

1. **Calculate Total Sum**: Sum all card points
2. **Edge Case**: If `k >= array length`, return total sum (can take all cards)
3. **Sliding Window**: Find minimum sum of `(n-k)` consecutive cards
   - Window size = `cardPoints.length - k`
   - Use two pointers to maintain the window
4. **Result**: Return `totalSum - minSubarray`

### Time Complexity: O(n)
### Space Complexity: O(1)