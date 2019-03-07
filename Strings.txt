APIs:
substring(startIndex, endIndex);


StringBuffer::
reverser();





// reverse string
    public void reverseString(char[] s) {
        for (int start = 0, end = s.length - 1; start < end; start++, end--) {
            char ch = s[start];
            s[start] = s[end];
            s[end] = ch;
        }
        
        return;
    }
===
// reverse the integer
    public int reverse(int x) {
        int result = 0;
        while (x != 0) {
            int lastdigit = x % 10;
            
            int newresult = result * 10 + lastdigit;
            if (((newresult - lastdigit) / 10) != result) return 0;
            result = newresult;
            
            x = x / 10;
        }
        
        return result;
    }
===

class Solution {
    public String shortestPalindrome(String s) {
        if(s == null || s.length() <= 1) return s;
        int i = s.length()-1;
        for(; i >=0; i--){
            if(isPD(s, 0, i)) break;
        }
        if( i == s.length()-1) return s;
        return reverse(s.substring(i+1)) + s;
        
        
    }
    
    private String reverse(String s){
        char[] cs = s.toCharArray();
        int i = 0, j = s.length() -1;
        while(i<j){
            char t= cs[i];
            cs[i] = cs[j];
            cs[j] = t;
            i++;
            j--;
        }
        return String.valueOf(cs);
    }
    
    private boolean isPD(String s, int start, int end){
        int i = start;
        int j = end;
        while(i < j){
            if(s.charAt(i) != s.charAt(j)) return false;
            i++;
            j--;
        }
        return true;
    }
}
