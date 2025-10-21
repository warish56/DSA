# Construct Binary Tree from Preorder and Inorder Traversal

**Problem Link:** [Construct Binary Tree from Preorder and Inorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/description/)

**Solution Link:** [Solution Video](https://neetcode.io/solutions/construct-binary-tree-from-preorder-and-inorder-traversal)

## Similar Problems

- [Construct Binary Tree from Inorder and Postorder Traversal](https://leetcode.com/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)
- [Construct Binary Search Tree from Preorder Traversal](https://leetcode.com/problems/construct-binary-search-tree-from-preorder-traversal/)
- [Serialize and Deserialize Binary Tree](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/submissions/1797635096/)

## Core Idea (Plain Words)

1. **Given**: Both preorder and inorder traversal arrays of a binary tree
2. **Goal**: Reconstruct the original binary tree from these traversals

3. **Key Observations**:
   - **Preorder traversal**: `[Root, ____Left Subtree____, ____Right Subtree____]`
   - **Inorder traversal**: `[____Left Subtree____, Root, ____Right Subtree____]`

4. **Algorithm**:
   - The first element in preorder is always the **root** of the current (sub)tree
   - Find this root element's index in the inorder array
   - All elements to the **left** of this index in inorder form the **left subtree**
   - All elements to the **right** of this index in inorder form the **right subtree**
   - Recursively apply the same logic to build left and right subtrees by:
     - Slicing the preorder array to get corresponding elements
     - Slicing the inorder array to get corresponding elements




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

    public int findIdx(int arr[], int target){
        for(int i=0; i<arr.length; i++){
            if(arr[i] == target){
                return i;
            }
        }
        return -1;
    }

    public TreeNode buildTree(int[] preorder, int[] inorder) {

        if(preorder.length == 0 || inorder.length == 0){
            return null;
        }

        TreeNode root = new TreeNode(preorder[0]);
        int mid = findIdx(inorder, preorder[0]);

        root.left = buildTree(Arrays.copyOfRange(preorder, 1, mid+1), Arrays.copyOfRange(inorder, 0, mid));
        root.right = buildTree(Arrays.copyOfRange(preorder, mid+1, preorder.length), Arrays.copyOfRange(inorder, mid+1, inorder.length));

        return root;

        
    }
}

```

### Explanation

#### Step-by-Step Walkthrough

Let's say we have:
- **Preorder**: `[3, 9, 20, 15, 7]`
- **Inorder**: `[9, 3, 15, 20, 7]`

**Step 1**: Build the root
- First element in preorder is `3` → This is the root
- Find `3` in inorder at index `1`
- Left of index 1: `[9]` → left subtree elements
- Right of index 1: `[15, 20, 7]` → right subtree elements

**Step 2**: Build left subtree
- Preorder for left: `[9]` (from index 1 to 1+1)
- Inorder for left: `[9]` (from index 0 to 1)
- First element is `9` → Create node with value 9
- No more elements → This is a leaf node

**Step 3**: Build right subtree
- Preorder for right: `[20, 15, 7]` (from index 2 to end)
- Inorder for right: `[15, 20, 7]` (from index 2 to end)
- First element is `20` → Create node with value 20
- Find `20` in inorder at index `1` (relative to right subtree)
- Continue recursively...

Final tree:
```
     3
    / \
   9  20
      / \
     15  7
```

#### Code Breakdown

1. **Base Case**: If preorder or inorder is empty, return `null`

2. **Create Root**: The first element of preorder is always the root
   ```java
   TreeNode root = new TreeNode(preorder[0]);
   ```

3. **Find Middle Index**: Locate root in inorder to split left and right subtrees
   ```java
   int mid = findIdx(inorder, preorder[0]);
   ```

4. **Recursively Build Subtrees**:
   - **Left subtree**: 
     - Preorder: from index `1` to `mid+1` (excluding root, taking `mid` elements)
     - Inorder: from index `0` to `mid` (everything before root)
   
   - **Right subtree**:
     - Preorder: from `mid+1` to end (remaining elements after left subtree)
     - Inorder: from `mid+1` to end (everything after root)

### Complexity Analysis

- **Time Complexity**: `O(n²)` where n is the number of nodes
  - We visit each node once: `O(n)`
  - For each node, we search for it in inorder array: `O(n)`
  - Array copying with `Arrays.copyOfRange`: `O(n)` per recursive call
  - Total: `O(n²)`
  
  *Note: This can be optimized to `O(n)` by using a HashMap to store inorder indices*

- **Space Complexity**: `O(n)`
  - Recursion stack depth: `O(h)` where h is height (worst case: `O(n)`)
  - Array copies at each level: `O(n)` total across all recursive calls
  - Total: `O(n)`

### Optimization Note

The current solution can be optimized by:
1. Using a HashMap to store `value → index` mapping for inorder array (eliminates `findIdx` search)
2. Using index pointers instead of creating array copies
3. This would reduce time complexity to `O(n)`
