
# Focus on Crux
## *** One important learning :: Develop your algorithm from within. i.e. build the heart of the algorithm as it is natural for brain to focus on the crux of the solutions. And then build the traversal around it. Traversal is universal i.e a bunch of for loops around it. 
## For e.g., even for the tiny atoi, the crux is building the result = result * 10 + (char - '0'). once you catch  this, it is not a big deal to loop it and is not hard mostly. 
## For word ladder, the crux is to get the neighbors, it is not a big deal to write the queue BFS logic over it.

# Dig the hidden property to optimize
## For all substring Palindrome problems, remember to use the property of Palindrome to reduce the time complexity. i.e. 2^n is the obvious solution for subsets but Palindrome property can be used to reduce it to linear. For every letter, think the possibility of building the palindrome by expanding (1) with that as centre or (2) left-biased or (3) right-biased.

## the same could be extended for BST. Use the BST property while traversing.

# Think Local (i.e for each node)
## Think what could be done for each node in the inputs. for eg., take line feeding algm or rain water trapping, the ultimate is to narrow down the logic to be applied for each node, which at the result yields the desired result.

# Think Dual Storage (no harm in it)
## LRU cache is a perfect example. You need O(1) access and least-recently used cache invalidation tech. No way to acheive this with one DS. think dual. One HashSet and one LL and connect it wisely by storing the LL node in HashSet. cool.

## the same way as told by Jay to random get() but O(1) put. so hashset and arraylist. Hashset stores the index and arraylist stores the data.

## minStack. you need to maintain the stack property but need to get the min value in O(1). so, dual stack approach.

# For DFS, think about caching. it is called DP

# Optimize. sounds hard but focus on loops. it is enough.

# Revisit TODO and FIXME. Listen to the inner self and write TODO/README sooner

Programming wisdom

# never replace r,c with x,y variables. remember r indicates row whereas x indicates column. thought process will be screwed