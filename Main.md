## Abstract Data Type
- provides only interface for that ds to adhere to
- no specific to language or implementation
- List, Queue, Map
   - Dynamic Array, linked list, Linked list queue, stack based queue,Tree map, Hash map/table

## Computational Complexity Analysis
- How much Time?
- How much Space?
- Big-O notation
  - gives upper bound of the complexity in the worst case
  - Constant time = O(1)
  - Logarithmic time = O(log(n))
  - Linear time = O(n)
  - Linearithmic time = O(nlog(n))
  - Quadratic time = O(n*2)
  - Cubic time = O(n*3)
  - Exponential time = O(b*n), b > 1
  - Factorial time = O(n!)

- Big O properties
  - ignore constants in context of how big it is or small it is
  - ex: f(n) = 7log(n)*3 + 15n*2 + 2n*3 + 8 then O(f(n)) = O(n*3)
  - Finding all subsets of a set = O(2*n)
  - Finding all permutations of a string = O(n!)
  - Sorting using mergesort = O(nlog(n))
  - Iterating over all the cells in a matrix of size n by m = O(nm)