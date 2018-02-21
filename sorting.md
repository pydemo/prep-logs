 # Sorting
Q | Info 
--- | ---
 k-way merge|is the algorithm that takes as input k sorted arrays, each of size n. It outputs a single sorted array of all the elements.
 *|The first array is traversed k-1 times, the first as merge(array-1,array-2), the second as merge(merge(array-1, array-2), array-3) ... and so on. The result is k-1 merges with an average size of n*(k+1)/2 giving a complexity of O(n*(k^2-1)/2) which is O(nk^2).
 *|The problem can be solved in O(n log k) running time with O(n) space

```Python
def xor(l):
    r = 0
    for v in l: r ^= v
    return r
```
It gains the name "exclusive or" because the meaning of "or" is ambiguous when both operands are true;
```
>>> 0 ^ 0
0
>>> 0 ^ 1
1
>>> 1 ^ 0
1
>>> 1 ^ 1
0
```
XOR is used in RAID 3–6 for creating parity information. For example, RAID can "back up" bytes 100111002 and 011011002 from two (or more) hard drives by XORing the just mentioned bytes, resulting in (111100002) and writing it to another drive. Under this method, if any one of the three hard drives are lost, the lost byte can be re-created by XORing bytes from the remaining drives. For instance, if the drive containing 011011002 is lost, 100111002 and 111100002 can be XORed to recover the lost byte.
# Algos
Q | Info 
--- | ---
Number |It takes log(n) bits to represent number N in binary
Fermat witness| n power (p-1) = 1 mod p
Fermat liar| not prime
