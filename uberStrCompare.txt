/**
 * Write a comparator that takes two strings and returns a standard integer value: 
 *          something negative if the first string is "smaller,"
 *          zero if they are "equal," 
 *          and something positive if the first string is "larger." 
 * We want to agree with the standard comparator for all cases except one: 
 * if we encounter a consecutive string of integers, we want to read it for its numeric value,
 * and use that as the comparison.
 * 
 * For instance, in the standard string comparator, "a10b" comes before "a2b", because 'a' == 'a' and '1' < '2'.
 * In our string ordering, I want to reverse this, instead parsing it so that we see 'a' == 'a', but 10 > 2.
 *
 */
import java.io.*;
import java.util.*;
import java.text.*;
import java.math.*;
import java.util.regex.*;

public class Solution {
    public static void main(String args[] ) throws Exception {
        /* Enter your code here. Read input from STDIN. Print output to STDOUT */
        
        // hi
        // log("result=" + compareTo("a10b", "a2b"));
        //log("result=" + compareTo("a2b", "a10b"));
        
        log("result=" + compareTo("2222222", "2222"));
        
        // log("atoi=124 as " + atoi("a124a".toCharArray(), 0));
    }
    
    static void log(String msg) {
        System.out.println(msg);
    }
    
    static public int compareTo(String s1, String s2) {
        // TODO handle edge cases
        if (s1 == null && s2 == null) return 0;
        if (s1 == null ^ s2 == null) {
            if (s1 == null) return 1;
            else return -1;
        }
        
        if (s1.equals(s2)) return 0;
        
        int first = 0;
        int second = 0;
        
        char[] s1Array = s1.toCharArray();
        char[] s2Array = s2.toCharArray();
        
        
        while (first < s1.length() && second < s2.length()) {
            // check for character
            char fch = s1Array[first];
            char sch = s2Array[second];
            
            boolean isFirstDigit = Character.isDigit(s1Array[first]);
            boolean isSecondDigit = Character.isDigit(s2Array[second]);
            
            if (!Character.isDigit(s1Array[first]) && !Character.isDigit(s2Array[second])) { // both are alpha
                if (fch == sch) {
                    first ++;
                    second ++;
                    continue;
                }
                return (fch - sch);
            }
            
            if (isFirstDigit && isSecondDigit) { // number parsing and update first and second accordinlty
            // use atoi
                int firstNumber = atoi(s1Array, first);
                int secondNumber = atoi(s2Array, second);
                
                if (firstNumber != secondNumber) {
                    return firstNumber - secondNumber;
                }
                
                // incr first and second
                int numberLength = String.valueOf(firstNumber).length();
                first += numberLength;
                second += numberLength;
            
                continue;    
            }
            
            // one of them differs
            // character gets more preference
            return (isFirstDigit) ? -1 : 1;
            
        }
        
        return (first > second) ? 1 : -1;
    }
    
    // FIXME integer overflow
    private static int atoi(char[] s, int index) {
        int result = 0;
        while(index < s.length && Character.isDigit(s[index])) {
            result = result * 10 + (s[index++] - '0');
        }
        
        return result;
    }
    
}