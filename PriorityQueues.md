## Priority Queues (PQs) with an interlude on heaps
- It is an Abstract Data Type (ADT) that operates similar to a normal queue except that each element has a certain priority.
- The priority of the elements in the priority queue determine the order in which elements are removed from the PQ.
- Priority queues only supports comparable data, meaning the data inserted into the priority queue must be able to be ordered in some way either from least to greatest or greatest to least. This is so that we are able to assign relative priorities to each element.

- Operations
  - poll()
  - add()

## Heap
- A heap is a tree based data structure that satisfies the HEAP INVARIANT (also called heap property): If A is a parent of B then A is ordered with respect to B for all nodes A, B in the heap.
- The value of parent of is always greater than or equal to value of child node for all nodes and vice versa.
- Max heaps and Min heaps
- Max Heap: parent node is always greater than the children nodes
- Min Heap: parent node is always less than the children nodes
- Both of the above are binary heaps, as they have two children.
- heaps is a canonical ds underlying in priority queues.
- Heaps can have any number of branches.
- All heaps must be tress, No cycles in between nodes.
- single node is a heap and a tree.

- Uses (Priority Queues)
  - Used in certain implementations for Dijkstra's shortest path algorithms
  - Anytime you need to dynamically fetch the 'next best' or 'next worst' element.
  - used in Huffman coding (which is often used for lossless data compression).
  - Best First Search (BFS) algorithms such as A* use PQs to continuously grab the next most promising node.
  - Used by Minimum Spanning Tree (MST) algorithms.

- Complexity Analysis (PQs with binary heap)
  - Binary heap construction: O(n)
  - polling: O(log(n))
  - peeking: O(1)
  - adding: O(log(n))
  - naive removing: O(n) // removing element which is not a root element
  - advanced removing with help from a hash table*: O(log(n))
  - naive contains: O(n)
  - contains check with help of a hash table*: O(1)

  * Using a hash table to help optimize these operations does take up linear space and also adds some overhead to the binary heap implementation.

- Turning min PQ to max PQ
  - Often the standard lib of most programming languages only provide a min PQ which sorts by smallest elements first, but sometimes we need a max PQ.
  - Since elements in a PQ are comparable they implement some sort of comparable interface which we can simply negate to achieve a max heap.

- Adding elements to binary heap
  - Priority queues are usually implemented with heaps since this gives them the best possible time complexity.
  - The PQ is an Abstract Data Type (ADT), hence heaps are not the only way to implement PQs. As an example, we could use an unsorted list, but this would not give us the best possible time complexity.
  - can be done by number of different types of heaps such as
    - Binary heap
    - Fibonacci heap
    - Binomial heap
    - Pairing heap
  - Binary heap: it is a binary tree that supports the heap invariant. In a binary tree every node has EXACTLY two children.
  - leaf nodes have children which are called as null children.
  - Complete Binary Tree Property
    - A complete binary tree is a tree in which at every level, except possibly the last is completely filled and all the nodes are as far left as possible.
    - if i is the parent node index (index of an array) then insertion formula for
      - left child index: 2*i + 1
      - right child index: 2*i + 2

- Removing elements from binary heap
  - root node: hightest priority.
  - when we remove root is called polling, while removing we don't need to search for it's index.
  - swap with the last element of the binary tree.
  - In case of tie in children, select the left node.
  -  search, when not removing root node as we don't know the index of the element.
  - polling: O(long(n))
  - removing: O(n) // random node, not root

- Removing elements from binary heap in O(log(n))
  - The inefficiency of the removal algorithm comes from the fact that we have to perform a linear search to find out where an element is indexed at. What if instead we did a lookup using a hashtable to find out where a node is indexed at?
  - A hashtable provides a constant time lookup and update for a mapping from a key (the node value to a value (the index).
  - Caveat: what if there are two or more nodes with the same value?  - Dealing with multiple value problem:
    - Instead of mapping one value to one position we will map one value to multiple positions. We can maintain a Set or Tree Set of indexes for which a particular node value (key) maps to.
  - If we want to remove a repeated node in our heap, which node do we remove and does it matter which one we pick?
    - No, it doesn't matter which node we remove as long as we satisfy the heap invariant in the end.
