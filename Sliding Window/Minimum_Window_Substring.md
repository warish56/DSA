
# Minimum Window Substring

**Problem Link:** [LeetCode - Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring/description/?utm_source=chatgpt.com)

## Similar Problems
1. **Problem Link:** [Find All Anagrams in a String](https://leetcode.com/problems/find-all-anagrams-in-a-string/?utm_source=chatgpt.com)

## Description

This problem teaches us how to keep track of all required elements along with their duplicates using the sliding window technique.

### Key Concepts:

1. We need to keep track of whether our window contains all elements of string `t`
2. We use the concept of `need` and `have` variables:
   - `need` represents the total characters in string `t`
   - `have` represents the count of characters we have collected so far

## Solution

### Approach
- Use sliding window technique with two pointers (`i` and `j`)
- Maintain character frequency maps for both strings
- Track `need` (total characters needed) and `have` (characters collected)
- Expand window when `have < need`, contract when `have == need`

### Code

```java
class Solution {
    public String minWindow(String s, String t) {
        HashMap<Character, Integer> tmap = new HashMap<Character, Integer>();
        HashMap<Character, Integer> smap = new HashMap<Character, Integer>();

        // Build frequency map for string t
        for(char c: t.toCharArray()){
            tmap.put(c, tmap.getOrDefault(c, 0)+1);
        }

        int need = t.length();
        int have = 0;
        int minLen = Integer.MAX_VALUE;
        int start = 0;

        int i = 0, j = 0;

        while(j < s.length()){
            char c = s.charAt(j);
            smap.put(c, smap.getOrDefault(c, 0)+1);
            
            // If character is in t and we haven't exceeded its required count, increment have
            if(tmap.containsKey(c) && smap.get(c) <= tmap.get(c)){
                have++;
            }

            if(have < need){
                j++;
            } else if(have == need){
                // Once we have all required characters, try to shrink window from left
                while(have == need){
                    int newLen = j-i+1;
                    if(newLen < minLen){
                        start = i;
                        minLen = newLen;
                    }
                    
                    char c1 = s.charAt(i);
                    smap.put(c1, smap.get(c1)-1);

                    // If removing this character breaks our requirement, decrement have
                    if(tmap.containsKey(c1) && smap.get(c1) < tmap.get(c1)){
                        have--;
                    }

                    i++;
                }
                j++;
            }
        }

        return minLen == Integer.MAX_VALUE ? "" : s.substring(start, start+minLen);
    }
}
```

### Explanation

1. **Initialization**: Create frequency maps for both strings and initialize tracking variables
2. **Expansion Phase**: Move right pointer (`j`) and update character counts
3. **Contraction Phase**: When all characters are found (`have == need`), try to minimize window by moving left pointer (`i`)
4. **Result**: Return the minimum window substring or empty string if no valid window exists