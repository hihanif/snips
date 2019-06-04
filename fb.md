# FB qns

## 712. Minimum ASCII Delete Sum for Two Strings

* good to catch the similarities with LCS (Longest common subsequence)
* missed the ASCII part. (did the regular s.charAt(i) - 'a' where substration is not needed)
* a dry run of every method is important. even asciiSum could be caught if dry run is done

```
class Solution {
    public int minimumDeleteSum(String s1, String s2) {
        return asciiSum(s1) + asciiSum(s2) - 2*fcs(s1, s2, 0, 0, new int[s1.length()][s2.length()]);
    }
    
    int asciiSum(String s) {
        int sum = 0;
        for (int i = 0; i < s.length(); i++) {
            sum += s.charAt(i);
        }
        log("sum=" + sum);
        return sum;
    }
    
    void log(String msg) {
        System.out.println(msg);
    }
    
    // find common subsequence
    int fcs(String s1, String s2, int i, int j, int[][] memo) {
        if (i == s1.length() || j == s2.length()) 
            return 0;
        
        if (memo[i][j] != 0) return memo[i][j];
        
        int result = 0;
        if (s1.charAt(i) == s2.charAt(j)) {
            result = s1.charAt(i) + fcs(s1, s2, i + 1, j + 1, memo);
        } else {
            result = Math.max(fcs(s1, s2, i, j + 1, memo), fcs(s1, s2, i + 1, j, memo));
        }
        
        memo[i][j] = result;
        
        return result;
    }
}
```