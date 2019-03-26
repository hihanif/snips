# DFS

## 101. Symmetric Tree
* checking if both one of two is NULL - use XOR operator
* DFS is better here. BFS sucks.

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) return true;
        return helper(root.left, root.right);
    }
    
    private boolean helper(TreeNode left, TreeNode right) {
        if (left == right) return true; // both null
        if (left == null ^ right == null) return false; // one is null
        
        // both are not null
        if (left.val != right.val) return false;
        return helper(left.left, right.right) && helper(left.right, right.left);
    }
}

## 490. The Maze

* look at the beauty of dirs. else, life will be hard

```java DFS
class Solution {
    public boolean hasPath(int[][] maze, int[] start, int[] destination) {
        if (start[0] == destination[0] && start[1] == destination[1])
            return true;
        
        int nRow = maze.length;
        int nCol = maze[0].length;
        
        int[][] dirs = {
            {0, 1}, {0, -1}, {-1, 0}, { 1, 0}
        };
        
        maze[start[0]][start[1]] = 2;
        
        for (int i = 0; i < 4; i ++) { // go in all four dirs
            int row = start[0];
            int col = start[1];
            
            while (row >= 0 && row < nRow && col >= 0 && col < nCol && maze[row][col] != 1) {
                row += dirs[i][0];
                col += dirs[i][1];
            }
            
            // revert to the prev pos
            row -= dirs[i][0];
            col -= dirs[i][1];
             
            if (maze[row][col] == 2) continue; // visited already
            
            maze[row][col] = 2;
            
            if (hasPath(maze, new int[] {row, col}, destination)) {
                return true;
            }
        }
        
        return false;
    }
}
```
* PQ and visited table.

```java
class Solution {
    int[][] dir = new int[][]{{-1, 0}, {1, 0}, {0, 1}, {0, -1}};
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        boolean[][] visited = new boolean[maze.length][maze[0].length];
        PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> a[2] - b[2]);
        pq.add(new int[]{start[0], start[1], 0});
        while (pq.size() > 0) {
            int[] a = pq.poll();
            if (visited[a[0]][a[1]]) continue;
            visited[a[0]][a[1]] = true;
            if (a[0] == destination[0] && a[1] == destination[1]) return a[2];
            for (int[] d : dir) {
                int x = a[0], y = a[1], count = 0;
                while (isValid(x + d[0], y + d[1], maze)) {
                    x += d[0];
                    y += d[1];
                    count++;
                }
                if (visited[x][y]) continue;
                pq.add(new int[]{x, y, a[2] + count});
            }
        }
        return -1;
    }
    
    private boolean isValid(int x, int y, int[][] maze) {
        return x >= 0 && y >= 0 && x < maze.length && y < maze[0].length && maze[x][y] == 0;
    }
}
```
* same as above written by me.

```java
class Solution {
    public int shortestDistance(int[][] maze, int[] start, int[] destination) {
        if (start[0] == destination[0] && start[1] == destination[1]) return 0;
        
        PriorityQueue<int[]> pq = new PriorityQueue<>((o1,o2) -> o1[2] - o2[2]); // minheap
        boolean[][] visited = new boolean[maze.length][maze[0].length];
        
        pq.offer(new int[] {start[0], start[1], 0}); // [2] denotes teh distance
        
        while(!pq.isEmpty()) {
            int[] point = pq.poll();
            
            if (point[0] == destination[0] && point[1] == destination[1]) return point[2];
            
            if (visited[point[0]][point[1]]) continue;
            
            visited[point[0]][point[1]] = true;
            
            // visit all neighbors
            int[][] dirs = { {0, 1}, { 0, -1}, { -1, 0}, {1, 0}};
            for (int[] dir : dirs) {
                int r = point[0];
                int c = point[1];
                
                int count = 0;
                while (isValid(maze, r + dir[0], c + dir[1])) {
                    r += dir[0];
                    c += dir[1];
                    count ++;
                }
                
                if (visited[r][c]) continue;
                
                pq.add(new int[] {r, c, point[2] + count}); // update teh pq. PQ can have duplicates. no issues.
            }
        }
        
        return -1;
    }
    
    boolean isValid(int[][] maze, int r, int c) {
        if (r >= 0 && r < maze.length && c >= 0 && c < maze[0].length && maze[r][c] == 0)
            return true;
        return false;
    }
}
```