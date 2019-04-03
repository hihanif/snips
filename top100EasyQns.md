# Learnings of top 100 easy qns

## *** One important learning :: Develop your algorithm from within. i.e. build the heart of the algorithm as it is natural for brain to focus on the crux of the solutions. And then build the traversal around it. Traversal is universal i.e a bunch of for loops around it. 
## For e.g., even for the tiny atoi, the crux is building the result = result * 10 + (char - '0'). once you catch  this, it is not a big deal to loop it. 
## For word ladder, the crux is to get the neighbors, it is not a big deal to write the queue BFS logic over it.
## For all substring Palindrome problems, remember to use the property of Palindrome to reduce the time complexity. i.e. 2^n is the obvious solution for subsets but Palindrome property can be used to reduce it to linear. For every letter, think the possibility of building the palindrome by expanding (1) with that as centre or (2) left-biased or (3) right-biased.

### the same could be extended for BST. Use the BST property while traversing.

## Think what could be done for each node in the inputs. for eg., take line feeding algm or rain water trapping, the ultimate is to narrow down the logic to be applied for each node, which at the result yields the desired result.



# Arrays

## Two Sum

* oh my gosh! plenty of learning in this sum. Right reason why this is the very first problem in leetcode.
* first of all, the return type is missed. both indexes are needed
* hash set did not work, because index is needed.
* hash map failed bcoz duplicates are not allowed
* Arrays.sort failed bcoz index is screwed
* Finally copied the answer
* the beauty is being dynamic. instead of filling the map initially, fill while looping it.

1. wrong answer bcoz the first item of result should be the first index. oh my bad
2. int new[] - sucks (instead of new int[])
3. just used left/right instead of nums[left]/nums[right] - sleepy!

* learn learn learn to focus on the output
* learn to unit test the code by eyes. not by running.

## https://leetcode.com/explore/interview/card/top-interview-questions-easy/92/array/769/ Valid Sudoku

* good that maps are ok but failed in zone calculatino. used 9 instead of 3.

##   Intersection of Two Arrays II
* Set seems to be a good choice but failed since duplicates not allowed. Read the questions.
* Anytime Set has to be used, think about duplicate in the input of Set and also ordering is important or not

##  Move Zeroes
* logic is ok but failed to set zero at the end.

## rotate images
* really tricky. but listen to your inner mind. looping is not bad. figure out that 4 swaps are needed in loops. thats it.
* again looping is not bad. keep listening to your head.

# Strings

## myatoi
* missed to handle -42, +2
* instead of trim, just a while loop to skip spaces. but failed miserably due to missing i<len condn. as well as, when exit the loop, still failed to check the i<len condition. while loop is tricky for edge case validation. be careful
* Character.isDigit() is far inefficient since it further calls a bunch of internal APIs inside. but better to honor Unicode
* Be careful about the paranthesis while putting OR condition

## strstr
* pay attention to incr operatiosn like j++/i++ inside the loop stmts. possibility of double increment.

## count and say
* while appending to StringBuilder, dont mix numbers and characters. it gets added implicitly :(

## longest common prefix
* while nested looping, break just breaks only one loop

# LinkedLists

##   Remove Nth Node From End of List
* of course, remember the start dummy node. it is cool
* learn to visualize the n+1 fast move and fast!=null check

```java
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode start = new ListNode(0);
        start.next = head;
        
        ListNode slow = start, fast = start;
        while (n-- >= 0 && fast != null) { // one more jump
            fast = fast.next;
        }
        
        while (fast != null) {
            slow = slow.next;
            fast = fast.next;
        }
        
        slow.next = slow.next.next;
        return start.next;
    }
}
```

# Binary Tree
* null check of root is the freq mistake. Boundary condn handling. at least, add TODO


# DP
##   Climbing Stairs
* the recursive base condition can be multiple. here, for 1 return 1 and for 2 return 2 and zero for <1



