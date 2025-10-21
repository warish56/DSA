# Lowest Common Ancestor of a Binary Search Tree

**Problem Link:** [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)


## Similar Problems


## Core Idea (Plain Words)

- **Note Important**: In this question we are talking about Binary tree `not`  Binary `Search` Tree

1. we are given p and q nodes and we need to find the lowest parent of both which contains both the nodes
2. we just need to iterate over the root node
3. First check if its the one of the required node among the two
4. then check its both left and right node
5. ```

            if(
              (isRequiredNode && hasInLeft) || 
              (isRequiredNode && hasInRight) ||
              (hasInLeft && hasInRight)
            )
```
6. then its the LCA 





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


    
   /**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 */
class Solution {


    TreeNode ans = null;
     public boolean lowest(TreeNode root, TreeNode p, TreeNode q) {

        if(root == null){
            return false;
        }
    

        boolean isRequiredNode = root.val == p.val || root.val == q.val;

        

        boolean hasInLeft =  lowest(root.left, p, q);
        boolean hasInRight =  lowest(root.right, p, q);

        if(
            (isRequiredNode && hasInLeft) || 
            (isRequiredNode && hasInRight) ||
            (hasInLeft && hasInRight)
        ){
            if(ans == null){
                ans = root;
            }
             return true;
        }

        return isRequiredNode || hasInLeft || hasInRight;
        

    }

   
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        
        lowest(root, p, q);

        return ans;
      
    }
}
}

```

### Complexity Analysis

- **Time Complexity**: 

- **Space Complexity**:
