# Construct Binary Search Tree from Preorder Traversal

**Problem Link:** [Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)

## Core Idea (Plain Words)

1. **Given**: A preorder traversal array of a BST
2. **Goal**: Reconstruct the original BST from the preorder array

3. **Key Observations**:
   - **Preorder traversal**: `[Root, ____Left Subtree____, ____Right Subtree____]`
   - First element is always the **root**
   - For BST: All elements < root go to left subtree, all elements > root go to right subtree
   - We can use bounds (min, max) to ensure BST property during construction

4. **Algorithm**:
   - Use a recursive helper function with bounds
   - Track current index in preorder array
   - For each node:
     - Check if current value is within bounds (min < val < max)
     - If valid, create node and recursively build left and right subtrees
     - Update bounds: left subtree uses (min, root.val), right subtree uses (root.val, max)

## Solution

### Code

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private int index = 0;
    
    public TreeNode bstFromPreorder(int[] preorder) {
        return buildBST(preorder, Integer.MIN_VALUE, Integer.MAX_VALUE);
    }
    
    private TreeNode buildBST(int[] preorder, int min, int max) {
        // Base case: if we've processed all elements or current value is out of bounds
        if (index >= preorder.length || preorder[index] < min || preorder[index] > max) {
            return null;
        }
        
        // Current value is valid, create node
        int val = preorder[index++];
        TreeNode root = new TreeNode(val);
        
        // Build left subtree: all values must be < root.val
        root.left = buildBST(preorder, min, val);
        
        // Build right subtree: all values must be > root.val
        root.right = buildBST(preorder, val, max);
        
        return root;
    }
}
```

### Explanation

#### Step-by-Step Walkthrough

Let's say we have:
- **Preorder**: `[8, 5, 1, 7, 10, 12]`

**Step 1**: Build root
- First element is `8` → Create root with value 8
- Left subtree bounds: (MIN, 8) - all values < 8
- Right subtree bounds: (8, MAX) - all values > 8

**Step 2**: Build left subtree
- Next element is `5` → 5 < 8, valid → Create node 5
- Left of 5: bounds (MIN, 5)
- Next element is `1` → 1 < 5, valid → Create node 1
- Next element is `7` → 7 > 5 but 7 < 8, goes to right of 5
- Right of 5: bounds (5, 8)
- `7` is within (5, 8) → Create node 7

**Step 3**: Build right subtree
- Next element is `10` → 10 > 8, valid → Create node 10
- Left of 10: bounds (8, 10)
- Right of 10: bounds (10, MAX)
- Next element is `12` → 12 > 10, valid → Create node 12

Final tree:
```
        8
       / \
      5   10
     / \    \
    1   7    12
```

### Complexity Analysis

- **Time Complexity**: O(n) - We visit each element exactly once
- **Space Complexity**: O(n) - Recursion stack in worst case (skewed tree), O(log n) for balanced tree

