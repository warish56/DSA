# Task Scheduler (Greedy + Priority Queue)

**Problem Link:** [LeetCode - Task Scheduler](https://leetcode.com/problems/task-scheduler/description/)

## Similar Problems
- **Problem Link:** [Reorganize String](https://leetcode.com/problems/reorganize-string/)

## What is the problem asking?
Schedule CPU tasks with a cooldown: between two identical tasks there must be at least `n` intervals. Each interval executes at most one task or stays idle. Return the minimum number of intervals to finish all tasks.

## Core idea (plain words)
1. Count the frequency of each task.
2. Always run the currently most frequent available task (use a max-heap).
3. Use a cooldown queue to reinsert tasks when they become available again (`readyTime = time + n + 1`).
4. Advance time one interval at a time; if no task is ready, that interval is idle.

## Solution

### Approach
- Maintain a max-heap of tasks by remaining count.
- Maintain a FIFO cooldown queue of entries `[taskId, remainingCount, readyTime]`.
- At each time unit:
  - Move any entries from cooldown to heap whose `readyTime <= time`.
  - If heap not empty, pop one task, run it once (decrement count). If it still has remaining, push it into cooldown with `readyTime = time + n + 1`.
  - Increase `time` by 1.
- Stop when both heap and cooldown are empty. The `time` is the answer.

### Code (Simulation with Max-Heap + Cooldown)
```java
import java.util.*;

class Solution {
    public int leastInterval(char[] tasks, int n) {
        if (n == 0) {
            return tasks.length; // no cooldown required
        }

        Map<Character, Integer> freq = new HashMap<>();
        for (char c : tasks) {
            freq.put(c, freq.getOrDefault(c, 0) + 1);
        }

        PriorityQueue<int[]> heap = new PriorityQueue<>((a, b) -> b[1] - a[1]);
        for (Map.Entry<Character, Integer> e : freq.entrySet()) {
            heap.offer(new int[] { e.getKey() - 'A', e.getValue() });
        }

        // cooldown queue elements: [taskId, remainingCount, readyTime]
        Queue<int[]> cooldown = new ArrayDeque<>();
        int time = 0;

        while (!heap.isEmpty() || !cooldown.isEmpty()) {
            while (!cooldown.isEmpty() && cooldown.peek()[2] <= time) {
                int[] ready = cooldown.poll();
                heap.offer(new int[] { ready[0], ready[1] });
            }

            if (!heap.isEmpty()) {
                int[] task = heap.poll();
                int id = task[0];
                int remaining = task[1] - 1; // execute once
                if (remaining > 0) {
                    cooldown.offer(new int[] { id, remaining, time + n + 1 });
                }
            }
            // else idle

            time++;
        }

        return time;
    }
}
```

### Explanation
- The heap always selects the task with the highest remaining count, which reduces future idles.
- The cooldown queue ensures the same task is not scheduled before `n` intervals pass.
- Each loop iteration accounts for one interval (task or idle), hence `time` is the total intervals.

### Alternative Greedy Formula (O(26))
- Let `maxCount` be the maximum frequency among tasks.
- Let `numMax` be how many tasks have frequency `maxCount`.
- Answer is:
  - `max(tasks.length, (maxCount - 1) * (n + 1) + numMax)`

This counts the frames formed by the most frequent tasks and fills them.

### Time and Space Complexity
- Simulation:
  - Time: `O(T log U)`, where `T` is total intervals and `U` is number of unique tasks (≤ 26). In practice ~`O(tasks.length)`.
  - Space: `O(U)`.
- Formula:
  - Time: `O(U)` (or `O(26)`), Space: `O(U)`.
