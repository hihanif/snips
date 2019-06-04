# 79. Word Search
* missed the if check in the first double for loop
* recursive - think about visited to avoid infinite loop. also about resetting it.
* recursive - base condition. very important.
* Look at the last snippet of looping all directions. just for ||s. cooler than for loop with dirs[].

```java
class Solution {
    public boolean exist(char[][] board, String word) {
        boolean[][] visited = new boolean[board.length][board[0].length];
        
        for (int i = 0; i < board.length ; i++) {
            for (int j = 0; j < board[0].length; j++) {
                if (helper(board, word.toCharArray(), 0, i, j, visited))
                    return true;
            }
        }
        
        return false;
    }
    
    private boolean helper(char[][] board, char[] word, int index, int i, int j, boolean[][] visited) {
        if (index == word.length) return true;
        if (i < 0 || i >= board.length || j < 0 || j >= board[0].length) return false;
        
        if (visited[i][j]) return false;
        
        if (word[index] != board[i][j]) return false;
        
        visited[i][j] = true;
        
        // try all neighbors
        boolean result = helper(board, word, index + 1, i, j+1, visited) ||
            helper(board, word, index + 1, i, j -1, visited)  ||
            helper(board, word, index + 1, i+1, j , visited) ||
            helper(board, word, index + 1, i-1, j, visited);
        
        // reset
        visited[i][j] = false;
        
        return result;
    }
}
```

253. Meeting Rooms II
* line feeding algorithm. Split the start and end, sort by time.
* Conflict resolution :: if both start || both end || start-end || end-start

56. Merge Intervals

* no need to think from only the line-feeding algorithm perspecitve i.e. split of start/end
* just sort by start and keep comparing the end with start.
* this same strategy does not work for minConferenceRoom because if 2 meetings overlaps on 1 meetings, it will not find. here, it isnot needed since we are just merging. 


```javaclass Solution {
    public int[][] merge(int[][] intervals) {
        // TODO validate inputs
        if (intervals == null || intervals.length == 0) return new int[0][];
        Arrays.sort(intervals, (o1, o2) -> o1[0] - o2[0]);
        
        int start = intervals[0][0];
        int end = intervals[0][1];
        
        List<int[]> result = new ArrayList<>();
        
        for (int[] i : intervals) {
            if (i[0] <= end) { // overlapping interval
                end = Math.max(end, i[1]); 
            } else { // non overlapping new interval
                result.add(new int[] { start, end});
                start = i[0];
                end = i[1];
            }
        }
        result.add(new int[] { start, end});
        
        return result.toArray(new int[0][]);
    }
}
```

252. Meeting Rooms

* no need to spilt start/end.
* just loop and think what decision to be taken for each item
* here check if the new item'start < the prev end. thats it.

class Solution {
    public boolean canAttendMeetings(int[][] intervals) {
        if (intervals == null || intervals.length == 0) return true;
        Arrays.sort(intervals, (o1,o2) -> o1[0] - o2[0]);
        
        int end = intervals[0][1];
        for (int i = 1; i < intervals.length ; i++) {
            if (intervals[i][0] < end) return false;
            else end = intervals[i][1];
        }
        
        return true;
    }
}
