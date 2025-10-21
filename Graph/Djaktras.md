

# Dijkstra's Algorithm

**Problem Link:** [LeetCode - Network Delay Time](https://leetcode.com/problems/network-delay-time/description/?utm_source=chatgpt.com)

## Similar Problems

**Problem Link:** [Shortest Path in Binary Matrix](https://leetcode.com/problems/shortest-path-in-binary-matrix/description/)



## What problem does Dijkstra solve?

Goal: shortest distance from one start node to every other node in a weighted graph.
Rule: all edge weights must be non-negative. (If you have negatives, use Bellman–Ford.)

## Core idea (plain words)

Start at the source with distance 0; everyone else is ∞.
Always expand the closest not-yet-finalized node next (use a min-heap / PriorityQueue).
When you “relax” an edge (u → v, w), you try to improve dist[v] with dist[u] + w.
Once a node is popped from the heap with its best distance, it’s final (can’t get cheaper later with non-negative weights).




## Solution

### Approach
\- Build a graph from edges and set all distances to ∞ except source `k = 0`
\- Use a min-heap (PriorityQueue) to always expand the closest node so far
\- Pop node; if the popped distance is worse than the best known, skip (stale entry)
\- Relax edges: if `dist[u] + w < dist[v]`, update and push `(v, dist[v])` into heap
\- If any node is unreachable at the end, return `-1`; otherwise return the max distance

### Code

```java

class Solution {
    public int networkDelayTime(int[][] times, int n, int k) {

        HashMap<Integer, HashSet<Integer>> map = new HashMap<Integer, HashSet<Integer>>();
        HashMap<String,Integer> timeMap = new HashMap<String,Integer>();

        for(int edge[]: times){
            HashSet<Integer> temp = map.getOrDefault(edge[0], new HashSet<Integer>());
            temp.add(edge[1]);
            map.put(edge[0], temp);
            timeMap.put(""+edge[0]+","+edge[1], edge[2]);
        }


        PriorityQueue<int[]> q = new PriorityQueue<>((a,b) -> a[1] - b[1]);
        int time[] = new int[n+1];
        for(int i=1; i<=n; i++){
            time[i] = Integer.MAX_VALUE;
        }

        q.add(new int[]{k, 0});
        time[k] = 0;


        while(q.size() > 0){
            int data[] = q.poll();
            int currNode = data[0];
            int tillTime = data[1];
            // stale entry guard
            if(tillTime > time[currNode]){
                continue;
            }
            if(!map.containsKey(currNode)){
                continue;
            }

            HashSet<Integer> neighbours = map.get(currNode);

            for(int neighbour: neighbours){
                String timeKey = ""+currNode+","+neighbour;
                int timeTaken = timeMap.get(timeKey);
                int totalTime = timeTaken + tillTime;
                if(
                   totalTime < time[neighbour] 
                ){
                    time[neighbour] = totalTime;
                    q.add(new int[]{neighbour, totalTime});
                }
            }        
        }


        int max = Integer.MIN_VALUE;
        for(int i=1; i<=n; i++){
            if(time[i] == Integer.MAX_VALUE){
               // System.out.println("==not== "+i);
                return -1;
            }
            max = Math.max(max, time[i]);
        }

        return max;

        
    }
}
```
\n### Explanation
\n1. Initialize distances, push source `(k, 0)` to heap
2. Always expand the node with the smallest known time; skip stale entries
3. Relax outgoing edges to improve neighbor times
4. If any node remains at ∞, graph is not fully reachable, return `-1`
5. Otherwise, the answer is the maximum time among all nodes
\n### Time Complexity
\- O((N + E) log N) due to heap operations
\n### Space Complexity
\- O(N + E) for graph, distances, and heap