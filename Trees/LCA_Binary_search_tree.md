# Lowest Common Ancestor of a Binary Search Tree

**Problem Link:** [Lowest Common Ancestor of a Binary Search Tree](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/description/)


## Similar Problems


## Core Idea (Plain Words)

- **Note Important**: In this question we are talking about Binary Search tree not just Binary Tree

1. we are given p and q nodes and we need to find the lowest parent of both which contains both the nodes
2. we just need to iterate over the root node
3. if both the nodes are greater than the root node then shift to right
4. if both the nodes are lesser than the root node then shift to left
5. else it is the LCA




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


    
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        TreeNode curr = root;

        while(curr != null){
            if(p.val > root.val && q.val > root.val){
              curr = root.right;
            }else if(p.val < root.val && q.val < root.val){
              curr = root.left;
            }else{
              return root;
            }
        }

    }
}

```

### Complexity Analysis

- **Time Complexity**: 

- **Space Complexity**:
