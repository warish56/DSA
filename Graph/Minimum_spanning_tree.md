# Kruskal's Minimum Spanning Tree Algorithm

**Problem Link:** [Connecting Cities With Minimum Cost](https://leetcode.ca/2019-01-08-1135-Connecting-Cities-With-Minimum-Cost/)

## Similar Problems

**Problem Link:** [Redundant Connection](https://leetcode.com/problems/redundant-connection/description/)

## Problem Statement
Given a graph with weighted edges, find the minimum cost to connect all nodes such that:
- All nodes are connected (no isolated components)
- The total cost is minimized
- No cycles are formed

## What problem does Kruskal's algorithm solve?

**Goal:** To connect all the nodes with the minimum total cost

**NOTE IMPORTANT:** This algorithm is also used to `find any cycle` in the graph as well


## Core Idea (Plain Words)
1. **Sort all edges by weight** - Start with the cheapest edges first
2. **Use Union-Find (Disjoint Set)** - Keep track of which nodes are already connected
3. **Add edges greedily** - For each edge, if it doesn't create a cycle, add it to the MST
4. **Stop when all nodes are connected** - We need exactly (n-1) edges for n nodes

## Solution

### Approach
1. **Sort edges** by weight in ascending order
2. **Initialize Union-Find** data structure where each node is its own parent
3. **Iterate through sorted edges**:
   - Check if adding this edge creates a cycle (nodes already connected)
   - If no cycle, add the edge and union the two components
   - Keep track of total cost and number of edges added
4. **Return result**:
   - If we successfully added (n-1) edges, return total cost
   - Otherwise, return -1 (graph is not connected)

### Code

```java
class Solution {
    private int[] p;

    public int minimumCost(int n, int[][] connections) {
        // Sort edges by weight (cost)
        Arrays.sort(connections, Comparator.comparingInt(a -> a[2]));
        
        // Initialize Union-Find: each node is its own parent
        p = new int[n];
        for (int i = 0; i < n; ++i) {
            p[i] = i;
        }
        
        int ans = 0; // Total cost
        for (int[] e : connections) {
            int x = e[0] - 1, y = e[1] - 1, cost = e[2];
            
            // Check if adding this edge creates a cycle
            if (find(x) == find(y)) {
                continue; // Skip this edge (would create cycle)
            }
            
            // Add edge to MST
            p[find(x)] = find(y); // Union the two components
            ans += cost;
            
            // Early termination: if we've added n-1 edges, we're done
            if (--n == 1) {
                return ans;
            }
        }
        return -1; // Graph is not connected
    }

    // Find with path compression
    private int find(int x) {
        if (p[x] != x) {
            p[x] = find(p[x]); // Path compression
        }
        return p[x];
    }
}
```

## Algorithm Analysis

### Time Complexity
- **O(E log E)** - Sorting edges dominates the time complexity
- **O(E α(V))** - Union-Find operations with path compression
- **Overall: O(E log E)** where E is the number of edges

### Space Complexity
- **O(V)** - Union-Find parent array
- **Overall: O(V)** where V is the number of vertices

### Key Insights
1. **Greedy Strategy**: Always pick the smallest available edge that doesn't create a cycle
2. **Union-Find**: Efficiently tracks connected components and detects cycles
3. **Path Compression**: Optimizes future find operations
4. **Early Termination**: Stop as soon as we have (n-1) edges

## Example Walkthrough
```
Graph: 4 nodes, edges: (1,2,3), (1,3,4), (2,3,5), (2,4,6), (3,4,7)

Step 1: Sort edges by weight
- (1,2,3) - cost 3
- (1,3,4) - cost 4  
- (2,3,5) - cost 5
- (2,4,6) - cost 6
- (3,4,7) - cost 7

Step 2: Process edges
- Add (1,2,3): cost = 3, edges = 1
- Add (1,3,4): cost = 7, edges = 2  
- Skip (2,3,5): would create cycle
- Add (2,4,6): cost = 13, edges = 3

Result: Minimum cost = 13
```

## When to Use Kruskal's Algorithm
- **Sparse graphs** (few edges relative to nodes)
- **Need to find MST** of a weighted graph
- **Graph connectivity problems**
- **Network design** problems