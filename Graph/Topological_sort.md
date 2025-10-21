

# Topological Sorting Algorithm

**Problem Link:** [Course Schedule II](https://leetcode.com/problems/course-schedule-ii/submissions/1784968016/)

## Similar Problems

1. **Problem Link:** [Course Schedule ](https://leetcode.com/problems/course-schedule/)
2. **Problem Link:** [Course Schedule ](https://leetcode.com/problems/redundant-connection/description/)




## What problem does Topological Sorting algorithm solves?

Goal: To find the order in which the all the nodes with least inbound edges comes first and then keeps on increasing

## Core idea (plain words)

1. create a adjency list of the nodes
2. create an indegree map for every nodes, inititalized to the number of indegrees for every Node
3. Initially add all the nodes with Zero indegrees in the queue
4. Loop over the queue until its not finished
5. after you popped an item in the queue, go through its adjency list and reduce the inDegree map for that node
6. if after reducing the inDegree for that it comes to zero then add it back to the queue
7. At the end go through the list of the nodes you added while traversing the queue





## Solution

### Code

```java


class Solution {



    public int[] findOrder(int numCourses, int[][] prerequisites) {

     HashMap<Integer, ArrayList<Integer>> adjList = new HashMap<Integer, ArrayList<Integer>>();
     int inDegMap[] = new int[numCourses];

     for(int preq[]: prerequisites){
        int curr = preq[0];
        int before = preq[1];

        ArrayList<Integer> temp = adjList.getOrDefault(curr, new ArrayList<Integer>());
        temp.add(before);

        inDegMap[before]++;
        adjList.put(curr, temp);

     }

     Queue<Integer> q = new LinkedList<>();

     for(int i=0; i<inDegMap.length; i++){
        if(inDegMap[i] == 0){
            q.add(i);
        }
     }

    ArrayList<Integer> res = new ArrayList<Integer>();

     while(q.size() > 0){
        int course = q.poll();
        res.add(course);

        ArrayList<Integer> tempList = adjList.getOrDefault(course, new ArrayList<Integer>());

        for(int preqCourse: tempList){
            inDegMap[preqCourse]--;

            if(inDegMap[preqCourse] == 0){
                q.add(preqCourse);
            }
        }
     }

     if(res.size() != numCourses){
        return new int[0];
     }


    Collections.reverse(res);
    int temp[]=new int[res.size()];

    for(int i=0; i<res.size(); i++){
        temp[i]=res.get(i);
    }
     return temp;

        
    }
}




   
```


\n### Explanation
\n1. Initialize distances, push source `(k, 0)` to heap
5. Otherwise, the answer is the maximum time among all nodes
\n### Time Complexity
\- (N + E)
\n### Space Complexity
\- O(N + E) for graph, distances, and heap