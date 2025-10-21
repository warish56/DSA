# Bellman Ford Algorithm

**YouTube Link:** [Bellman Ford algorithm](https://www.youtube.com/watch?v=FtN3BYH2Zes&list=PLDN4rrl48XKpZkf03iYFl-O29szjTrs_O&index=53)

**Problem Link:** [Cheapest Flights Within K Stops](https://leetcode.com/problems/cheapest-flights-within-k-stops/)

---

## What problem does Bellman Ford solve?

**Goal:** Find the shortest distance from one start node to every other node in a weighted graph. 

**Key Feature:** Unlike Dijkstra's algorithm, Bellman-Ford can handle **negative edge weights**.

---

## Core Idea (Plain Words)

1. **Relaxation Process:** Start by relaxing every edge in the graph, and repeat this process **(V-1)** times, where **V** is the number of vertices.

2. **Why V-1 iterations?** In a graph with V vertices, the longest simple path between any two vertices contains at most **(V-1)** edges. Therefore, after (V-1) iterations of relaxing all edges, we are guaranteed to find the shortest path.

3. **Edge Relaxation:** For an edge (u → v) with weight w, if `distance[u] + w < distance[v]`, then update `distance[v] = distance[u] + w`.

---

## Solution

### Approach

1. **Initialize Distances:**
   - Create a `prices[]` array of size `n` to store the minimum cost to reach each node
   - Set all values to `Integer.MAX_VALUE` (infinity) initially
   - Set `prices[src] = 0` since the cost to reach the source is 0

2. **Relax Edges (V-1) times:**
   - For each iteration (up to n-1 times):
     - For each flight (edge) in the graph:
       - Extract source, destination, and cost
       - Skip if the source node hasn't been reached yet (`prices[source] == Integer.MAX_VALUE`)
       - Calculate `newPrice = prices[source] + cost`
       - If `newPrice < prices[destination]`, update `prices[destination] = newPrice`

3. **Return Result:**
   - If `prices[dst]` is still `Integer.MAX_VALUE`, return `-1` (destination unreachable)
   - Otherwise, return `prices[dst]` (minimum cost to reach destination)

**Note:** For the "Cheapest Flights Within K Stops" problem specifically, the loop should run **(K+1)** times instead of (n-1) times to respect the constraint of at most K stops.

### Code

```java
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        
        int prices[] = new int[n];
        Arrays.fill(prices, Integer.MAX_VALUE);
        prices[src] = 0;

        for(int i=0; i<n-1; i++){
            for(int flight[]: flights){
                int source = flight[0];
                int dest = flight[1];
                int dist = flight[2];
                
                if (prices[source] == Integer.MAX_VALUE) {
                    continue;
                }

                int newPrice = prices[source] + dist;
                if(newPrice < prices[dest]){
                    prices[dest] = newPrice;
                }
            }
        }

        return prices[dst] == Integer.MAX_VALUE ? -1 : prices[dst];
    }
}
```

### Explanation

**Step-by-Step Walkthrough:**

1. **Initialization (Lines 36-38):**
   - We create a `prices` array where `prices[i]` represents the minimum cost to reach city `i`
   - All cities start with infinite cost except the source city, which has cost 0

2. **Edge Relaxation Loop (Lines 40-56):**
   - **Outer Loop:** Runs (n-1) times, which is sufficient for standard Bellman-Ford
   - **Inner Loop:** Iterates through all flights (edges)
   
3. **Relaxation Logic (Lines 42-54):**
   - For each flight, we extract the source, destination, and cost
   - **Optimization (Lines 46-48):** Skip unreachable source nodes to avoid integer overflow
   - **Update Rule (Lines 51-53):** If going through this flight gives a cheaper path to the destination, update the cost

4. **Return Statement (Line 58):**
   - If destination is unreachable (still has infinite cost), return -1
   - Otherwise, return the minimum cost found

**Time Complexity:** O(E × V) where E is the number of edges (flights) and V is the number of vertices (cities)

**Space Complexity:** O(V) for the prices array

**Important Note:** The current implementation doesn't utilize the parameter `k` (maximum stops). To properly solve the "Cheapest Flights Within K Stops" problem, the outer loop should iterate `(k+1)` times instead of `(n-1)` times, and we should use a temporary array to avoid using updated values in the same iteration.

---

## When to Use Bellman-Ford?

- ✅ Graph has **negative edge weights**
- ✅ Need to find shortest path from **one source to all nodes**
- ✅ Can detect **negative cycles** (with one extra iteration)
- ❌ Not ideal for graphs with only positive weights (use Dijkstra's instead - it's faster)
