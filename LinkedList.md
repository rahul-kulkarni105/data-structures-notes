## Singly and Doubly Linked list
- Linked list is a sequential list of nodes that holds data which points to other nodes also containing data.
- Last node, points to null. Always has a null reference.
- Uses
  - Queue, stack, list implementations
  - creating circular lists
  - can easily model real world objects such as trains
  - Used in separate chaining, which is present certain hashtable implementations to deal with hashing collisions.
  - used in implementations of adjacency lists of graphs.

- Terminology
  - Head: first node
  - Tail: last node
  - Pointer: reference to another node
  - Node: an object containing data

- Singly vs Doubly
  - Singly contains only contains pointer to next node, doubly has pointer to next AND previous node.
  - singly less memory, because of less pointers but can't easily access previous nodes, Doubly can be traversed backwards but takes 2X memory.

- Implementation
  - Singly Insertion
    - Create a new pointer pointing to head. almost always the first step.
    - seek up to but not the node we want to remove
    - ready to insert, create next node
    - point current node to another (new) and point new node to point to next one to the current node.
  - Doubly Insertion
    - Same exact steps until we seek up to the desired node.
    - create new node
    - point new nodes pointer to next node and previous pointer to current node
    - point next nodes previous pointer to new node
    - current nodes pointer to new node
    - changing 4 pointers here.

  - Singly deletion
    - trick is to use 2 pointers
    - create pointers for two nodes, traversal 1 points to head and trav 2 to next node
    - advanced trav 2 to node to remove and also advance trav 1
    - stop when node to remove found.
    - trav 2 is now one after the removed
    - trav 1 is before the removed node.
    - trav1 next pointer set to trav 2.
    - clean up memory.
  - Doubly deletion
    - seek upt o node we wish to remove
    - need only 1 traversal pointer
    - trav at beginning, and seek until we hit desired node.
    - remove the node where trav is at, set travs previous node point to next and travs next node point to previous node.

- Complexity for singly and doubly
  - Search: O(n) O(n)
  - Insert at head: O(1) O(1)
  - Insert at tail: O(1) O(1)
  - remove at head: O(1) O(1)
  - remove at tail: O(n) O(1) // Possible question
  - Insert in middle: O(n) O(n)

