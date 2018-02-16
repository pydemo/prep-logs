 # Sorting
Q | Info 
--- | ---
 k-way merge|is the algorithm that takes as input k sorted arrays, each of size n. It outputs a single sorted array of all the elements.
 __ __ |The first array is traversed k-1 times, the first as merge(array-1,array-2), the second as merge(merge(array-1, array-2), array-3) ... and so on. The result is k-1 merges with an average size of n*(k+1)/2 giving a complexity of O(n*(k^2-1)/2) which is O(nk^2).
 -|The problem can be solved in O(n log k) running time with O(n) space
