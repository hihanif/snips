# DP

* it takes time to understand the concept of subproblems
* but mostly, try to memoize an recursive solution. then it is DP
* recursion is quite easy to think what are the possible options to move and then memoize. it is a top down appr. but the memo array is filled from bottom to top
* bottom up is the reverse of top down but filled from top to bottom ie.(00 to mn)
* https://www.youtube.com/watch?v=sSno9rV8Rhg

## Longest common subsequence of strings

* recursion with memo

```java
class Solution {
    public int minDistance(String word1, String word2) {
        memo = new int[word1.length()][word2.length()];
        for (int[] i : memo)
            Arrays.fill(i, -1);
        
        int common = helper(word1, word2, 0, 0);    
        log("count=" + count);
        
        return word1.length() - common + word2.length() - common;
    }
    
    void log(String msg) {
        System.out.println(msg);
    }
    
    int[][] memo;
    int count;
    
    int helper(String w1, String w2, int i1, int i2) {
        count ++;
        
        if (w1.length() == i1 || w2.length() == i2)
            return 0;

        if (memo[i1][i2] != -1) return memo[i1][i2];
        
        char ch1 = w1.charAt(i1);
        char ch2 = w2.charAt(i2);
        
        int res = 0;
        if (ch1 == ch2) {
            res = 1 + helper(w1, w2, i1 + 1, i2 + 1);
        } else {
            res = Math.max(helper(w1, w2, i1, i2 + 1), helper(w1, w2, i1 + 1, i2));
        }
        
        memo[i1][i2] = res;
        return res;
     }
}
``` 

* DP bottom up appr

```java
class Solution {
    public int minDistance(String word1, String word2) {
        int[][] memo = new int[word1.length() + 1][word2.length() + 1];
        
        for (int i = 1; i <= word1.length(); i++) {
            for (int j = 1; j <= word2.length(); j++) {
                if (word1.charAt(i-1) == word2.charAt(j-1)) {
                    memo[i][j] = 1 + memo[i-1][j-1];
                } else {
                    memo[i][j] = Math.max(memo[i-1][j], memo[i][j-1]);
                }
            }
        }
        
        int common = memo[word1.length()][word2.length()];
        log("common=" + common);
        
        return word1.length() - common + word2.length() - common;
    }
    
    void log(String msg) {
        // System.out.println(msg);
    }
}
```

* beautiful coding.

```java
 public Interval[] intervalIntersection(Interval[] A, Interval[] B) {
        int minend = 0, maxstart = 0;
        List<Interval> res = new ArrayList<>();
        int i = 0, j = 0;
        while(i<A.length && j < B.length){
            Interval a = A[i];
            Interval b = B[j];
            minend = Math.min(a.end,b.end);
            maxstart = Math.max(a.start,b.start);
            if(minend >= maxstart){
                res.add(new Interval(maxstart,minend));
            }
            if(a.end == minend)i++;
            if(b.end == minend)j++;
        }
        return res.toArray(new Interval[res.size()]);
    }
```
