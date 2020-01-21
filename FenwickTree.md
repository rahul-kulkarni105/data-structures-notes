## Fenwick tree (Binary Indexed Tree)
- A Fenwick Tree (also called Binary Indexed Tree) is a data structure that supports sum range queries as well as setting values in a static array and getting the value of the prefix sum up some index efficiently.

- Complexity analysis
  - construction: O(n)
  - point update: O(log(n))
  - range sum: O(log(n))
  - range update: O(log(n))
  - adding index: N/A
  - removing index: N/A

- Fenwick Tree Range queries
  - Unlike regular array, in a FT a specific cell is responsible for other cells as well.
  - The position of the least significant bit (LSB) determines the range of responsibility that cell has to the cells below itself.
  - Examples:
    - consider numbers 1 to 16
    - Index 12 in binary is: 1010₂. LSB is at position 3, then this index is responsible for 2³⁻¹ = 4 cells below itself.
    - Index 10 in binary is: 1100₂. LSB is at position 2, then this index is responsible for 2²⁻¹ = 2 cell (can be itself).
    - All odd numbers have their first least significant bit (Binary representation of the number and the right most digit is 1, not 0) set in the ones position, so they are only responsible for themselves.
    - Number with their least significant bit in the second position have a range of two.
    - Number with their least significant bit in the third position have a range of four.
    - Number with their least significant bit in the fourth position have a range of eight.
    - Number with their least significant bit in the fifth position have a range of sixteen.
  - In a FT we may compute the prefix sum up to a certain index, which ultimately lets us perform range sum queries.
  - Idea: Suppose you want to find the prefix sum of [1, i], then you start at i and cascade downwards until you reach zero adding the value at each of the indices you encounter.
  - Example:
    - finding prefix sum of index 1 - 7 inclusive. (Usually everything in FT is inclusive)
    - starts from 7 down to 0 in LSB representation.
    - The downward cascade is based on how much range responsibility that numbers binary representation has. Basically starting from right in LSB what position do you encounter 1 first.
    - sum = A[7] + A[6] + A[4]
    - finding prefix sum for 11
    - sum = A[11] + A[10] + A[8]
    - finding prefix sum for 4
    - sum = A[4]

    - interval sum between [11, 15]
    - first we compute prefix sum of [1, 15] then we will compute the prefix sum of [1, 11) and get the difference.
    // [) this means NOT INCLUSIVE. We want the value at position 11.
    - sum of [1, 15] = A[15] + A[14] + A[12] + A[8]
    - sum of [1, 11] = A[10] + A[8]
    range sum: (A[15] + A[14] + A[12] + A[8]) - (A[10] + A[8])
  - Notice that in the worst case the cell we're querying has a binary representation of all ones (numbers of the form 2ⁿ-1)
  - Hence, it's easy to see that in the worst case a range query might make us have to do two queries that cost log₂(n) operations.

- Fenwick Tree Point Updates
  - Point updates are the opposite of above way of doing things.
  - We want to ADD the LSB to propagate the value UP to the cells responsible for us.
  - If we need to add x to some position, which cells we need to modify?:
    - in case of adding to position 6
    - 6 = 0110₂, 0110₂+0010₂ = 1000₂
    - 8 = 1000₂, 1000₂+1000₂ = 10000₂
    - 16 = 1000₂
    - required updates:
      A[6] = A[6] + x
      A[8] = A[8] + x
      A[16] = A[16] + x
  - Algorithm:
    - to update the cell at index i in the FT of size N
    function add(i, x):
      while i < N:
        tree[i] = tree[i] + x
        i = i + LSB(i)
    - where LSB returns the value of the least significant bit.
    - for example LSB(12) = 4 because 12 to the base 10 = 1100₂ and the least significant bit of 1100₂ is 100₂, or 4 in base ten.

- FT construction
  - Naive construction: Let A be an array of values. For each element in A at index i do a point update on the Fenwick tree with a value if A[i]. There are n elements and each point update takes O(log(n)) for a total of O(log(n))
  - Linear construction: Idea: Add the value in the current cell to the immediate cell that is responsible for us. This resembles what we did for point updates but only one cell at a time.
  - This will make the 'cascading' effect in range queries possible by propagating the value in each cell throughout the tree.
  - Let i be the current index, the immediate cell above us is at position j give by j: i + LSB(i).
  - Ignore updating j if index is out of bounds.
  - After constructing fenwick tree, we can now perform point and range query updates as required.
  - Construction algorithm: Make sure values are 1-based.
    function construct(values):
      N := length(values)
      // clone the values array since we are doing in place operations
      tree = deepCopy(values)
      for i = 1,2,3,... N:
        j := i + LSB(i)
        if j < N:
          tree[j] = tree[j] + tree[i]
      return tree
