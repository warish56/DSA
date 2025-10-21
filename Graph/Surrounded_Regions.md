

# Surrounded Regions

**Problem Link:** [LeetCode - Surrounded Regions](https://leetcode.com/problems/surrounded-regions/submissions/1783456420/)




## Problem Description

Here we need to check if we can wrap any surrounded cells or not



## Solution
Instead of checking if we can wrap the cells with the `X` or not we can check if we can reach any edge from the particualr `O` cell, if yes then we cannot wrap that cell group else we can

### Code

```java

class Solution {


    ArrayList<int[]> tempList = new ArrayList<int[]>();

    public boolean canReachEdge(char[][] board, int i, int j, HashSet<String> visited){
        
        if(
            (
                (i <= 0 || i >= board.length-1) ||
                (j <= 0 || j >= board[0].length-1)
            ) &&
            board[i][j] == 'O'
        ){
            return true;
        }
        
        
        
        String key = ""+i+","+j;

        if(visited.contains(key)){
            return false;
        }

        tempList.add(new int[]{i, j});
        visited.add(key);

        int directions[][] = {
            {1, 0},
            {-1, 0},
            {0, 1},
            {0, -1}
        };

        boolean can = false;
        for(int direction[]: directions){
            int nr = direction[0]+i;
            int nc = direction[1]+j;

            String nKey = ""+nr+","+nc;

            if(
                nr >=0 && nr < board.length &&
                nc >=0 && nc < board[0].length &&
                !visited.contains(nKey) &&
                board[nr][nc] == 'O'
            ){
                can = can || canReachEdge(board, nr, nc,visited);
            }

        }

        return can;
    }
 

    public void solve(char[][] board) {


        for(int i=0; i<board.length; i++){
            for(int j=0; j<board[0].length; j++){

                if(
                    board[i][j] == 'O' &&
                    i > 0 && i < board.length-1 &&
                    j > 0 && j < board[0].length-1
                   
                ){

                    boolean can = canReachEdge(board, i, j, new HashSet<String>());
                    if(!can){
                        for(int pos[]:tempList){
                            board[pos[0]][pos[1]] = 'X';
                        }
                    }

                    tempList.clear();
                    
                }
            }
        }

      
        
    }
}


```