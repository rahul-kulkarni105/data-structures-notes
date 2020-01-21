## Indexed Priority Queue (IPQ)
- An Indexed Priority Queue is a traditional priority queue variant which on top of the regular PQ operations supports quick updates and deletions of key-value pairs.
- It is very important to be able to dynamically update the priority value of certain keys.
- The Indexed Priority Queue (IPQ) data structure lets us do this efficiently. The first step to using an IPQ is to assign index values to all keys forming a bidirectional mapping.
- This uses a bidirectional hashtable.
- Note: This assumes you know how many keys you will have in your IPQ, but this mapping can be constructed dynamically as well.
- Reason for mapping:
  - Why are we mapping keys to indexes in the domain [0, N)?
  - Typically priority queues are implemented as heaps under the hood which internally use arrays which we want to facilitate indexing into.
  - Note: Often the keys themselves are integers in the range [0, N) so there's no need for the mapping, but it's handy to be able to support any type of key (like names).

- IPQ ADT Interface
  - If 'k' is the key we want to update first get the key's index: ki = map[k], then use 'ki' with the IPQ
    - delete(ki)
    - valueOf(ki)
    - contains(ki)
    - peekMinKeyIndex()
    - pollMinKeyIndex()
    - peekMinValue()
    - pollMinValue()
    - insert(ki, value)
    - update(ki, value)
    - decreaseKey(ki, value) // Specialized update operations
    - increaseKey(ki, value) // Specialized update operations

- Complexity analysis
  - IPQ as a binary heap
    operations: Indexed Binary Heap PQ
    - delete(ki): O(log(n))
    - valueOf(ki): O(1)
    - contains(ki): O(1)
    - peekMinKeyIndex(): O(1)
    - pollMinKeyIndex(): O(log(n))
    - peekMinValue(): O(1)
    - pollMinValue(): O(log(n))
    - insert(ki, value): O(log(n))
    - update(ki, value): O(log(n))
    - decreaseKey(ki, value): O(log(n))
    - increaseKey(ki, value): O(log(n))

- IPQ as binary heap
  - Suppose we have N people with different priorities we need to serve. Assume priorities can dynamically change and we always want to serve the person with the lowest priority.
  - To figure out who to serve next use a Min IPQ to sort by lowest value first.
  - Arbitrarily assign each person a unique index value between [0, n)
  - Give initial values to place inside IPQ. These will be maintained by the IPQ oce inserted. Note: These values can be any comparable value not only integers. strings, objects etc.
  - When we insert (ki, v) pairs into an IPQ we sort by the value associated with each key.
  - Example, if we are working with min heap, sort by smallest value.
  - To access the value foe any given key k, find it's key index (ki) and do a lookup in the values array maintained by the IPQ.
  - How do i find the index of the node for a particular key?:
  - The array pm (position map) we maintain to tell us the index of the node in the heap for a given key index (ki).
  - How do we go from knowing the position of a node to its key and ki value?:
  - To do that we also need to maintain an inverse lookup table denoted: im (Inverse Map)

- Operations
  - Same as regular PQ
  - Insertion:
    // Inserts a value into the min indexed binary heap. The key index must not already be in the heap and the value must not be null.
    function insert(ki, value):
      values[ki] = value
      // 's2' is the current size of the heap
      pm[ki] = sz
      im[sz] = ki
      swim(sz)
      sz = sz + 1
    - swim method:
      // swims up node o (zero based) until heap invariant is satisfied
      function swim(i):
        for (p = (i-1)/2; i > 0 and less(i, p)):
          swap(i, p)
          i = p
          p = (i-1)/2
      function swap(i, j):
        pm[im[j]] = i
        pm[im[i]] = j
        tmp = im[i]
        im[i] = im[j]
        im[j] = tmp
      function less(i, j):
        return values[im[i] < values[im[j]]]

- Polling and Removals
  - polling is still O(long(n)) in an IPQ, but removing is improved from O(n) in a traditional PQ to O(log(n)) since node position lookups are O(1) but repositioning is still O(log(n))
  - Removal:
    // Deletes the node with the key index ki in the heap. The key ki must exist and be present in the heap.
    function remove(ki):
      i = pm[ki]
      swap(i, sz)
      sz = sz -1
      sink(i)
      swim(i)
      values[ki] = null
      pm[ki] = -1
      im[sz] = -1
   - sink method:
    // sinks the node at index i by swapping itself with the smallest of the left or the right child node.
      function sink(i):
        while true:
          left = 2*i + 1
          right = 2*i + 2
          smallest = left

          if right < sz and less(right, left):
            smallest = right

          if left < sz and less(i, smallest):
            break

          swap(smallest, i)
          i = smallest

- Updates
  - similar to removals, updates in a min indexed binary heap also take O(log(n)) due to O(1) lookup time to find the node and O(log(n)) time to adjust where the key-value pair should appear in the heap.
  - Update method:
    // Updates the value of a key in the binary heap. The key index must exist and the value must not be null.
    function update(ki, value):
      i = pm[ki]
      values[ki] = value
      sink(i)
      swim(i)

- Decrease and Increase Key
  - In many applications (eg. Dijkstra's and Prims algorithm) it is often useful to only update a given key to make it's value either always smaller (or larger). In the event that a worse value is given the value in the OPQ should not be updated.
 In such situations it is useful to define a more restrictive form of update operations we call increaseKey(ki, v) and decreaseKey(ki, v)
 - methods:
  // For both these functions assume ki and value are valid inputs and we are dealing with a min indexed binary heap.
  function decreaseKey(ki, value):
    if less(value, values[ki]):
      values[ki] = value
      swim(pm[ki])

    function increaseKey(ki, value):
    if less(values[ki], value):
      values[ki] = value
      sink(pm[ki])