


# Floyd-Warshall Algorithm

**Problem Link:** [LeetCode - Find the City With the Smallest Number of Neighbors at a Threshold Distance](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/description/)

**YouTube Link:** [Striver - Floyd Warshall Algorithm](https://www.youtube.com/watch?v=YbY8cVwWAvw&list=PLgUwDviBIf0oE3gA41TKO2H5bHpPd7fzn&index=43)

## What problem does Floyd-Warshall solve?

Goal: shortest distance between **every pair** of nodes in a weighted graph (all-pairs shortest path).
Rule: edge weights can be negative, but there must be **no negative-weight cycles**. (If you only need single-source shortest paths, use Dijkstra or Bellman–Ford instead.)

## Core idea (plain words)

Build an N × N distance matrix initialized with direct edge weights (∞ where no edge exists, 0 on the diagonal).
Try every node `via` (0 … N-1) as a potential intermediate stop.
For each pair (i, j), check: is going i → via → j cheaper than the current best i → j?
If yes, update `dist[i][j] = dist[i][via] + dist[via][j]`.
After all N rounds, `dist[i][j]` holds the shortest path from i to j.



## Solution

### Approach
\- Initialize an N × N matrix: `dist[i][i] = 0`, everything else `= ∞`
\- Fill in direct edge weights (undirected → set both directions)
\- Run three nested loops: for each intermediate node `via`, for each pair `(i, j)`, relax through `via`
\- After Floyd-Warshall, count reachable cities within `distanceThreshold` for each node
\- Return the city with the smallest count (largest index on tie)

### Code

```java
class Solution {
    public int findTheCity(int n, int[][] edges, int distanceThreshold) {
        int INF = (int) 1e9;
        int[][] dist = new int[n][n];

        // initialize
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j) {
                    dist[i][j] = 0;
                } else {
                    dist[i][j] = INF;
                }
            }
        }

        // fill direct edges
        for (int[] edge : edges) {
            int u = edge[0];
            int v = edge[1];
            int w = edge[2];
            dist[u][v] = w;
            dist[v][u] = w;
        }

        // Floyd-Warshall
        for (int via = 0; via < n; via++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (dist[i][via] + dist[via][j] < dist[i][j]) {
                        dist[i][j] = dist[i][via] + dist[via][j];
                    }
                }
            }
        }

        int minReachable = Integer.MAX_VALUE;
        int answer = -1;

        for (int i = 0; i < n; i++) {
            int count = 0;

            for (int j = 0; j < n; j++) {
                if (i != j && dist[i][j] <= distanceThreshold) {
                    count++;
                }
            }

            if (count <= minReachable) {
                minReachable = count;
                answer = i;
            }
        }

        return answer;
    }
}
```

### Explanation

1. Build an N × N distance matrix; diagonal = 0, rest = ∞
2. Populate with given edge weights (both directions for undirected graph)
3. For each intermediate node `via`, try to improve every pair `(i, j)` through `via`
4. After all passes, `dist[i][j]` is the true shortest path between any two nodes
5. Count neighbors within threshold for each city; return the one with fewest (largest index on tie)

### Time Complexity
\- O(N³) — three nested loops over all nodes

### Space Complexity
\- O(N²) for the distance matrix
