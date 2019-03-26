# Queues and Stack

* And remember when you want to process the elements in order, using a queue might be a good choice.
* Learn to play around with mod (%) operator
* use head & tail notation
* move head/tail wisely. start from -1. Let head/tail point to teh current value. while enqueue, make room and add. while dequeue, take the element and move.

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
* missed to diff between size and capacity. Initially, the size is 1 & 2. it is not always 3. Size() vs Capacity.
* Prefer double over long. and no need to unnecesary cast. learn better progg.

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
* There are some cases where one does not need keep the visited hash set:
	* You are absolutely sure there is no cycle, for example, in tree traversal;
	* You do want to add the node to the queue multiple time
* trees dont have cycles. hence, BFS on trees may not keep visited nodes.

## Walls and Gates

* in the inner queue loop, i updaeted the row/col for every directions where it should not be :(
* dirs[] arrays is a very good technique to group the traversal and use it in for loop instead of naive if/else
* adding all the zeros in the beginning is clever since the shortest distance need not be recalculated/revisited again
	* if there are multiple origins, keep all origins to the queue in the very beginning itself
* DFS is ok  here but re-relaxing is an overhead. so, not good. You got it!
	* __i just feel that DFS is really terrible and a bad solution. Period.__

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

##   Open the Lock

* mistakes
	* updating a specific char is not as you think. no int to char. need to use a separate digits arrays
	* Failed first time. since deadendsSet is done only inside the loop. where as it is missed for the very first element :(
	* incrementing level can be tricky
		* either it means that the current node being processed at this level
		* it indicates the nodes being added to the queue
		* accordingly, the return value can be 'level+1' or 'level'.
		* Pay attention to the initial level -1 or 0.
	* deadendsSet could be reused as visited too
		

```java
class Solution {
    private char[] digits = {'0','1','2','3','4','5','6', '7', '8', '9'};

    private void log(String msg) {
        System.out.println(msg);
    }
    
    public int openLock(String[] deadends, String target) {
        // TODO try with char array for optimization
        Queue<String> q = new LinkedList<>();
        Set<String> deadendsSet = new HashSet<>(Arrays.asList(deadends));
        Set<String> visited = new HashSet<>();
        
        String[] strs = new String[2];
        
        if (deadendsSet.contains("0000")) return -1;
        
        q.add("0000");
        int level = -1;
        while (!q.isEmpty()) {
            int qsize = q.size();
            level ++; // it means that this level is being processed
            
            log("qsize=" + qsize);
            for(String s : q) {
                System.out.print(s + ",");
            }
            log("");
            
            while (qsize-- > 0) {
                String s = q.poll();
                            
                // try 2 combination for all chars
                char[] sArray = s.toCharArray();
                
                for (int i = 0; i < sArray.length; i++) {
                    char ch = sArray[i]; 
                    // roll back
                    sArray[i] = (ch == '0') ? digits[9] : digits[ch - '0' - 1];
                    strs[0] = new String(sArray);
                    
                    // roll front
                    sArray[i] = (ch == '9') ? digits[0] : digits[ch - '0' + 1];
                    strs[1] = new String(sArray);
                    
                    for (String str : strs) {
                        if (str.equals(target)) return level + 1;
                        
                        if (!deadendsSet.contains(str) && !visited.contains(str)) {
                            q.offer(str);
                            visited.add(str);
                        }
                    }
                    
                    sArray[i] = ch; // reverting to orig
                }
            }
            
        }
        
        return -1;
    }
}
```		

## 752. Open the Lock
## Wake up! Wake up! Wake up- Bidirectional BFS rocks.

We can consider bidirectional approach when-
* Both initial and goal states are unique and completely defined.
* __The branching factor is exactly the same in both directions.__
* \
* Completeness : Bidirectional search is complete if BFS is used in both searches.
* Optimality : It is optimal if BFS is used for search and paths have __uniform cost__.
* Time and Space Complexity : Time and space complexity is __O(b^{d/2})__
* https://efficientcodeblog.wordpress.com/2017/12/13/bidirectional-search-two-end-bfs/
* \
* visited is mostly necessary. almost always in BFS/DFS.

["0201","0101","0102","1212","2002"]
"0202"

### one way traversal - 72ms
qsize=1
0000,
qsize=8
9000,1000,0900,0100,0090,0010,0009,0001,
qsize=32
8000,0000,9900,9100,9090,9010,9009,9001,2000,1900,1100,1090,1010,1009,1001,0800,0990,0910,0909,0901,0200,0190,0110,0109,0080,0099,0091,0020,0019,0011,0008,0002,
qsize=86
7000,8900,8100,8090,8010,8009,8001,9800,9990,9910,9909,9901,9200,9190,9110,9109,9101,9080,9099,9091,9020,9019,9011,9008,9002,3000,2900,2100,2090,2010,2009,2001,1800,1990,1910,1909,1901,1200,1190,1110,1109,1101,1080,1099,1091,1020,1019,1011,1008,1002,0700,0890,0810,0809,0801,0980,0999,0991,0920,0919,0911,0908,0902,0300,0290,0210,0209,0180,0199,0191,0120,0119,0111,0108,0070,0089,0081,0098,0092,0030,0029,0021,0018,0012,0007,0003,
qsize=190
6000,7900,7100,7090,7010,7009,7001,8800,8990,8910,8909,8901,8200,8190,8110,8109,8101,8080,8099,8091,8020,8019,8011,8008,8002,9700,9890,9810,9809,9801,9980,9999,9991,9920,9919,9911,9908,9902,9300,9290,9210,9209,9201,9180,9199,9191,9120,9119,9111,9108,9102,9070,9089,9081,9098,9092,9030,9029,9021,9018,9012,9007,9003,4000,3900,3100,3090,3010,3009,3001,2800,2990,2910,2909,2901,2200,2190,2110,2109,2101,2080,2099,2091,2020,2019,2011,2008,1700,1890,1810,1809,1801,1980,1999,1991,1920,1919,1911,1908,1902,1300,1290,1210,1209,1201,1180,1199,1191,1120,1119,1111,1108,1102,1070,1089,1081,1098,1092,1030,1029,1021,1018,1012,1007,1003,0600,0790,0710,0709,0701,0880,0899,0891,0820,0819,0811,0808,0802,0970,0989,0981,0998,0992,0930,0929,0921,0918,0912,0907,0903,0400,0390,0310,0309,0301,0280,0299,0291,0220,0219,0211,0208,0170,0189,0181,0198,0192,0130,0129,0121,0118,0112,0107,0060,0079,0071,0088,0082,0097,0093,0040,0039,0031,0028,0022,0017,0013,0006,0103,0004,
qsize=__356__
5000,6900,6100,6090,6010,6009,6001,7800,7990,7910,7909,7901,7200,7190,7110,7109,7101,7080,7099,7091,7020,7019,7011,7008,7002,8700,8890,8810,8809,8801,8980,8999,8991,8920,8919,8911,8908,8902,8300,8290,8210,8209,8201,8180,8199,8191,8120,8119,8111,8108,8102,8070,8089,8081,8098,8092,8030,8029,8021,8018,8012,8007,8003,9600,9790,9710,9709,9701,9880,9899,9891,9820,9819,9811,9808,9802,9970,9989,9981,9998,9992,9930,9929,9921,9918,9912,9907,9903,9400,9390,9310,9309,9301,9280,9299,9291,9220,9219,9211,9208,9202,9170,9189,9181,9198,9192,9130,9129,9121,9118,9112,9107,9103,9060,9079,9071,9088,9082,9097,9093,9040,9039,9031,9028,9022,9017,9013,9006,9004,4900,4100,4090,4010,4009,4001,3800,3990,3910,3909,3901,3200,3190,3110,3109,3101,3080,3099,3091,3020,3019,3011,3008,3002,2700,2890,2810,2809,2801,2980,2999,2991,2920,2919,2911,2908,2902,2300,2290,2210,2209,2201,2180,2199,2191,2120,2119,2111,2108,2102,2070,2089,2081,2098,2092,2030,2029,2021,2018,2012,2007,1600,1790,1710,1709,1701,1880,1899,1891,1820,1819,...

### two way traversal - 19ms

qsize=1
0000,
qsize=1
0202,
qsize=8
0090,0100,0001,1000,0010,9000,0900,0009,
qsize=6
9202,0302,0203,1202,0212,0292,
qsize=32
9100,9001,0909,0190,0091,0200,0002,0000,0109,0901,0800,0008,1090,8000,0020,2000,1010,9010,0080,9090,0990,0099,0110,0011,1100,1001,9900,9009,0910,0019,1900,1009,
qsize=__29__
9102,9201,9302,9203,0392,0293,0192,0291,0402,0303,0204,0202,0301,0103,0222,2202,9212,8202,9292,0282,1292,0312,0213,1302,1203,0112,0211,1102,1201,

```java
class Solution {
    private char[] digits = {'0','1','2','3','4','5','6', '7', '8', '9'};

    private void log(String msg) {
        System.out.println(msg);
    }
    
    public int openLock(String[] deadends, String target) {
        // TODO try with char array for optimization
        // Queue<String> q = new LinkedList<>();
        Set<String> q = new HashSet<>();
        Set<String> qEnd = new HashSet<>();
        qEnd.add(target); // 2 end traversal
        
        Set<String> deadendsSet = new HashSet<>(Arrays.asList(deadends));
        // Set<String> visited = new HashSet<>();
        Set<String> visited = deadendsSet;
        
        String[] strs = new String[2];
        
        if (deadendsSet.contains("0000")) return -1;
        
        q.add("0000");
        int level = -1;
        while (!q.isEmpty() && !qEnd.isEmpty()) {
            int qsize = q.size();
            level ++; // it means that this level is being processed
            
            // log("qsize=" + qsize);
            // for(String s : q) {
            //     System.out.print(s + ",");
            // }
            // log("");
            
            Set<String> newQ = new HashSet<>();
            
            // while (qsize-- > 0) {
            //     String s = q.poll();
            for (String s : q) {
                // try 2 combination for all chars
                char[] sArray = s.toCharArray();
                
                for (int i = 0; i < sArray.length; i++) {
                    char ch = sArray[i]; 
                    // roll back
                    sArray[i] = (ch == '0') ? digits[9] : digits[ch - '0' - 1];
                    strs[0] = new String(sArray);
                    
                    // roll front
                    sArray[i] = (ch == '9') ? digits[0] : digits[ch - '0' + 1];
                    strs[1] = new String(sArray);
                    
                    for (String str : strs) {
                        // if (str.equals(target)) return level + 1;
                        if (qEnd.contains(str)) return level + 1; // 2 end traversal.
                        
                        // if (!deadendsSet.contains(str) && !visited.contains(str)) {
                        if (!deadendsSet.contains(str)) {
                            // q.offer(str);
                            newQ.add(str);
                            visited.add(str);
                        }
                        // if (!deadendsSet.contains(str)) newQ.add(str);
                    }
                    
                    sArray[i] = ch; // reverting to orig
                }
            }
            // swap the traversal & travel the next level.
            // q = qEnd; 
            // qEnd = newQ;
            if (newQ.size() <= qEnd.size()) {
                q = newQ;
            } else {
                q = qEnd;
                qEnd = newQ;
            }
        }
        
        return -1;
    }
}
```

## 279. Perfect Squares using Bidirectional BFS

* super cool. super fast.
* Mistakes:
	* swapSign is missed for the BFS from the targetEnd.
	* Yes, it is true that the braching factor is same but the impact is different. Where as for lock problem, it is same for both. While playing with numbers, we need to be double careful.
	* remember, even the swapSign with negative (-1) is a cool idea i steal from somewhere. :)
* though in this case, the visited set does not make any diff, keep an eye.


```java
class Solution {
    private void log(String msg) {
        System.out.println(msg);
    }
    
    public int numSquares(int n) {
        // find the edges
        List<Integer> edges = new ArrayList<>();
        for (int i = 1; i <= n ; i ++) {
            long val = i * i;
            if (val > n) break;
            if (val == n) return 1;
            edges.add((int)val);
        }
        
        Set<Integer> beginSet = new HashSet<>();
        Set<Integer> endSet = new HashSet<>();
        
       
        beginSet.add(n); 
        endSet.add(0); 
        
        Set<Integer> visited = new HashSet<>();
        visited.add(n);
        visited.add(0);
        
        int level = -1;
        int swapSign = -1;
        
        while (!beginSet.isEmpty() && !endSet.isEmpty())      {
            level ++;
            Set<Integer> newSet = new HashSet<>(); // stores all possible values in this level
        
            // log("beginSet size="  + beginSet.size() + " endSet size()=" + endSet.size());
            for (int curr : beginSet) {
                for (int e : edges) {
                    int newVal = curr + e*swapSign;
                    if (endSet.contains(newVal)) 
                        return level + 1;
                    if (newVal < 0) break; // all further edges will be greater only
                    // if (!visited.contains(newVal)) 
                        newSet.add(newVal);
                }
            }
            
            if (newSet.size() < endSet.size()) {
                beginSet = newSet;
            } else { // swap
                swapSign = -swapSign;
                beginSet = endSet;
                endSet = newSet;
            }
        }
        
        return -1;
    }
}
```
