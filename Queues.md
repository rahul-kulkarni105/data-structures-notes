## Queues
- Queue is a linear data structure which models a real world queue by having two primary operations, namely enqueue and dequeue.
- Has front and back end: called enqueuing, insert form BACK and remove from FRONT: called dequeuing.
- Enqueue = Offering = ADDING FROM THE BACK = back
- Dequeue = Polling = REMOVING FROM FRONT = front
- First In First Out (FIFO)

- Example
  - Enqueue()
  - Dequeue()

- Uses
  - model a real world queue, (Macdonald?!)
  - can be used to efficiently keep track of the x most recently added elements
  - Web server request management where you want first come first serve
  - Breadth First Search (BFS) graph traversal.

- Complexity analysis
  - Enqueue O(1)
  - Dequeue O(1)
  - Peeking O(1)
  - Contains O(n)
  - Removal O(n)
  - Is Empty O(1)

- Example
  - BFS graph traversal (Network)
  - BFS: start at a node and traverse entire graph, by visiting all the neighbors of the starting node and so on.
  - Arrays, Singly linked list and Doubly linked list usual ds used for queue.