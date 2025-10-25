# Disjoint Union Set (DSU) Algorithm

**Problem Link:** [Number of Islands](https://leetcode.com/problems/number-of-islands/description/)

## Problem Statement
Find the number of connected islands in a 2D grid where:
- `'1'` represents land
- `'0'` represents water
- Islands are connected horizontally or vertically (not diagonally)

## What problem does Disjoint Union Set solve?
Disjoint Union Set (DSU) is used to:
- Group connected nodes together into separate components
- Efficiently find if two nodes belong to the same component
- Merge two components when a connection is found
- Count the number of distinct components

## Core Idea (Plain Words)
1. **Initialize**: Each cell with land (`'1'`) starts as its own separate component
2. **Union**: When we find adjacent land cells, we merge their components
3. **Find**: We use path compression to efficiently find the root of each component
4. **Count**: The number of islands equals the number of distinct root components

The key insight is that we start with the maximum possible islands (all land cells) and reduce this count by 1 each time we find a connection between two separate components.

## Solution

### Approach
1. **First Pass**: Initialize all land cells as separate components and count them
2. **Second Pass**: For each land cell, check its 4 neighbors (up, down, left, right)
3. **Union Operation**: If a neighbor is also land and belongs to a different component, merge them
4. **Path Compression**: Optimize the find operation for better performance
5. **Return**: The final count of distinct components (islands)

### Code

```java


class Solution {

    HashMap<String, String> parentMap = new HashMap<String, String>();

    public String find(String node){
        if(parentMap.get(node) != node){
            parentMap.put(node, find(parentMap.get(node)));
        }

        return parentMap.get(node);
    }
  
    

    public int numIslands(char[][] grid) {

    int islands = 0;

        for(int i=0; i<grid.length; i++){
            for(int j=0; j<grid[0].length; j++){
                if(grid[i][j] == '1'){
                    String key = ""+i+","+j;
                    parentMap.put(key, key);
                    islands++; 
                }
            }
        }


        int edges[][] = {
            {1, 0},
            {-1, 0},
            {0, 1},
            {0,-1}
        };


        for(int i=0; i<grid.length; i++){
            for(int j=0; j<grid[0].length; j++){
                if(grid[i][j] == '0'){
                    continue;
                }

                String node1 = ""+i+","+j;

                for(int edge[]: edges){
                    int nr = i+edge[0];
                    int nc = j+edge[1];
                    String node2 = ""+nr+","+nc;

                    if(
                        nr>=0 && nr <grid.length &&
                        nc>=0 && nc <grid[0].length &&
                        grid[nr][nc] == '1'
                    ){
                        String ra = find(node1);
                        String rb = find(node2);

                        if(!ra.equals(rb)){
                            parentMap.put(ra, rb);
                            islands--;
                        }
                    }
                }
                    
                
            }
        }

        return islands;

    }
}
```
## Algorithm Analysis

### Time Complexity
- **Overall**: O(m × n × α(m × n)) where m and n are grid dimensions
- **Find Operation**: O(α(m × n)) with path compression (α is inverse Ackermann function, practically constant)
- **Union Operation**: O(α(m × n)) per operation
- **Grid Traversal**: O(m × n) for scanning the entire grid
- **Practical Complexity**: Nearly O(m × n) due to path compression optimization

### Space Complexity
- **HashMap Storage**: O(m × n) for storing parent relationships of all land cells
- **Recursion Stack**: O(m × n) in worst case for find operation (before path compression)
- **Overall**: O(m × n) space complexity

### Key Insights
1. **Path Compression**: The `find()` function uses path compression to flatten the tree structure, making future lookups faster
2. **Union by Rank**: While not implemented here, union by rank can further optimize the algorithm
3. **Component Counting**: Start with maximum possible components and decrement when merging
4. **Coordinate Mapping**: Use string keys like "i,j" to represent grid coordinates in the HashMap
5. **Boundary Checking**: Always validate neighbor coordinates before processing
6. **Incremental Processing**: Process each cell and its neighbors in a single pass

## Example Walkthrough

Let's trace through a simple 3×3 grid:

```
Grid:
1 1 0
0 1 0
1 1 1
```

**Step 1: Initialize all land cells**
- (0,0): parent = "0,0", islands = 1
- (0,1): parent = "0,1", islands = 2  
- (1,1): parent = "1,1", islands = 3
- (2,0): parent = "2,0", islands = 4
- (2,1): parent = "2,1", islands = 5
- (2,2): parent = "2,2", islands = 6

**Step 2: Process (0,0)**
- Check neighbor (0,1): Both land, different components
- Union: parent["0,0"] = "0,1", islands = 5

**Step 3: Process (0,1)**  
- Check neighbor (1,1): Both land, different components
- Union: parent["0,1"] = "1,1", islands = 4

**Step 4: Process (1,1)**
- No new connections found

**Step 5: Process (2,0)**
- Check neighbor (2,1): Both land, different components  
- Union: parent["2,0"] = "2,1", islands = 3

**Step 6: Process (2,1)**
- Check neighbor (2,2): Both land, different components
- Union: parent["2,1"] = "2,2", islands = 2

**Step 7: Process (2,2)**
- No new connections found

**Final Result**: 2 islands
- Island 1: (0,0) → (0,1) → (1,1) 
- Island 2: (2,0) → (2,1) → (2,2)

## Alternative Approaches

### DFS/BFS Approach
```java
// Time: O(m×n), Space: O(m×n) for recursion stack
public int numIslandsDFS(char[][] grid) {
    int count = 0;
    for (int i = 0; i < grid.length; i++) {
        for (int j = 0; j < grid[0].length; j++) {
            if (grid[i][j] == '1') {
                dfs(grid, i, j);
                count++;
            }
        }
    }
    return count;
}
```

### When to Use DSU vs DFS/BFS
- **Use DSU**: When you need to dynamically add/remove connections or when the problem involves multiple queries
- **Use DFS/BFS**: For simple counting problems like this one (more intuitive and slightly more efficient)

## Common Variations

1. **Number of Islands II**: Dynamic island creation
2. **Surrounded Regions**: Find regions surrounded by borders
3. **Friend Circles**: Social network connectivity
4. **Redundant Connection**: Find edges that create cycles

## Practice Problems

1. [Number of Islands II](https://leetcode.com/problems/number-of-islands-ii/)
2. [Surrounded Regions](https://leetcode.com/problems/surrounded-regions/)
3. [Friend Circles](https://leetcode.com/problems/friend-circles/)
4. [Redundant Connection](https://leetcode.com/problems/redundant-connection/)
5. [Accounts Merge](https://leetcode.com/problems/accounts-merge/)

## Summary

Disjoint Union Set is a powerful data structure for:
- **Dynamic Connectivity**: Efficiently handling union and find operations
- **Component Counting**: Tracking the number of connected components
- **Cycle Detection**: Identifying redundant connections in graphs
- **Network Analysis**: Social networks, computer networks, etc.

The key optimization techniques are:
- **Path Compression**: Flattening the tree structure during find operations
- **Union by Rank**: Attaching smaller trees to larger ones (not implemented in this example)
- **Coordinate Mapping**: Efficient representation of grid positions
