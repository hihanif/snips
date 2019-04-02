# Learnings of top 100 easy qns

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
