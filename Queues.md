# Queues and Stack

* And remember when you want to process the elements in order, using a queue might be a good choice.

```java
class MyCircularQueue {
        final int[] a;
        int front, rear = -1, len = 0;

        public MyCircularQueue(int k) { a = new int[k];}

        public boolean enQueue(int val) {
            if (!isFull()) {
                rear = (rear + 1) % a.length;
                a[rear] = val;
                len++;
                return true;
            } else return false;
        }

        public boolean deQueue() {
            if (!isEmpty()) {
                front = (front + 1) % a.length;
                len--;
                return true;
            } else return false;
        }

        public int Front() { return isEmpty() ? -1 : a[front];}

        public int Rear() {return isEmpty() ? -1 : a[rear];}

        public boolean isEmpty() { return len == 0;}

        public boolean isFull() { return len == a.length;}
    }
```
## 346. Moving Average from Data Stream

### mistakes i made in this tiny program
* first of all, sum is returned instead of avg.
* Integer overflow due to sum is not taken care. so double/long should always be used.
* missed to diff between size and capacity. 
* Prefer double over long. and no need to unnecesary cast.

```java
class MovingAverage {
    Queue<Integer> q;
    int capacity;
    //long sum;
	double sum;
    
    /** Initialize your data structure here. */
    public MovingAverage(int capacity) {
        q = new LinkedList<>();
        this.capacity = capacity;
    }
    
    public boolean isFull() {
        return q.size() == capacity;
    }
    public double next(int val) {
        if (isFull()) {
            //sum -= (long)q.remove();       
			sum -= q.remove();       
        }
        q.add(val);
        //sum += (long)val;
		sum += val;
		
        //return sum/(double)(q.size());
		return sum/q.size();
    }
}
```
## 359. Logger Rate Limiter

* no need to use System.timesInMills() API instead add 10 to the incoming msg
* missed two things
	* missed to access the msg member
	* missed to add to q :( while adding to set

```java
class Logger {

    class LogMessage {
        int ts;
        String msg;
        LogMessage(int ts, String msg) {
            this.ts = ts;
            this.msg = msg;
        }
    }
    
    Queue<LogMessage> q;
    Set<String> msgSet;
    
    /** Initialize your data structure here. */
    public Logger() {
        q = new LinkedList<>();
        msgSet = new HashSet<>();
    }
    
    /** Returns true if the message should be printed in the given timestamp, otherwise returns false.
        If this method returns false, the message will not be printed.
        The timestamp is in seconds granularity. */
    public boolean shouldPrintMessage(int timestamp, String message) {
        // prune it
        // int currTime = System.currentTimeMillis() / 1000;
        while (!q.isEmpty() && q.peek().ts+10 <= timestamp) {
            msgSet.remove(q.poll().msg);
        }
        
        // check and add
        if (msgSet.contains(message)) return false;
        q.add(new LogMessage(timestamp, message));
        msgSet.add(message);
        return true;
    }
}
```
# Queues and BFS

## Walls and Gates
* in the inner queue loop, i updaeted the row/col for every directions where it should not be :(
* dirs[] arrays is a very good technique to group the traversal and use it in for loop instead of naive if/else
* adding all the zeros in the beginning is clever since the shortest distance need not be recalculated

```java
class Solution {
    // BFS
    public void wallsAndGates(int[][] rooms) {
        Queue<int[]> queue = new LinkedList<>();
        
        for(int i = 0; i < rooms.length ; i++) {
            for (int j = 0; j < rooms[0].length ; j++) {
                if (rooms[i][j] == 0) queue.add(new int[] {i, j});
            }
        }
        
        // possible directions
        int[][] dirs = {
            {0, 1}, {0, -1}, {1, 0}, {-1, 0}
        };
        
        int level = 0;
        while (!queue.isEmpty()) {
            int size = queue.size();
            level ++;
            // log("size=" + size);
            while (size-- > 0) {
                int[] curr = queue.poll();
               
                for (int i = 0; i < dirs.length ; i++) {
                    int row = curr[0] + dirs[i][0];
                    int col = curr[1] + dirs[i][1];
                    
                    // boundary check
                    if (row < 0 || col < 0 || row >= rooms.length || col >= rooms[0].length) continue;
                    
                    // valid check                        
                    if (rooms[row][col] == Integer.MAX_VALUE) {
                        rooms[row][col] = level;
                        queue.add(new int[] {row, col});
                    }
                }
            }
        }
        
        return;
    }
    
    private void log(String msg) { System.out.println(msg); }
}
```

## same as above but DFS

* amazing brevity
* rooms[row][col] <= depth is a key idea to return to avoid looping.

```java
    // DFS
    public void wallsAndGates(int[][] rooms) {
        for (int i = 0; i < rooms.length ; i++)     {
            for (int j = 0; j < rooms[0].length; j++) {
                if (rooms[i][j] == 0) 
                    dfs(rooms, i, j, 0);
            }
        }
    }
    
    void dfs(int[][] rooms, int row, int col, int depth) {
        if (depth != 0 && (row < 0 || col < 0 || row >= rooms.length || col >= rooms[0].length || rooms[row][col] <= depth )) {
            return; // invalid or already visited to have shorter path
        }
        rooms[row][col] = depth;
        
        int[][] dirs = {{0, 1}, {0, -1}, {1, 0}, { -1, 0}};
        for (int i = 0; i < dirs.length; i ++) {
            dfs(rooms, row + dirs[i][0], col + dirs[i][1], depth + 1);
        }
    }
```
