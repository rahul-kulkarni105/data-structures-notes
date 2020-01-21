## Binary Trees and Binary Search Trees (BST)
- A tree is an unidirected graph which satisfies any of the following definitions:
  - An acyclic connected graph // no cycles.
  - A connected graph with N nodes and N-1 edges.
  - An graph in which any two vertices are connected by exactly one path.
- If we have a rooted tree then we will want to have a reference to the root node of our tree.
- It does not always matter which node is selected to be the root node because any node can root the tree.
- A child is a node extending from another node. A parent is the inverse of this.
- What is the parent of the root node?
  - Root node has no parent, although the parent of the root node may be itself in some cases. (eg: file system tree)
- A leaf node is anode with no children.
- A subtree is a tree entirely contained within another tree. They are usually denoted using triangles. It is entirely possible to have only one node in a subtree.
- A binary tree is a tree for which every node has AT MOST two child nodes.

- Binary Search Tree (BST)
  - it is a binary tree which satisfies the BST invariant: left subtree has smaller element and right subtree has larger elements.
  - BST operations allow for duplicate values, but most of the time we are only interested in having unique elements inside our tree.
  - Any data which is comparable and can be ordered.

- Uses
  - Binary Search Trees (BST)
    - Implementations of abstract data types, sets, maps
    - Red Black Trees
    - AVL Trees
    - Splay Trees etc..
  - used in implementation of binary heaps
  - syntax trees (used by compiler and calculators)
  - Treap - a probabilistic DS (uses a randomized BST)

- Complexity Analysis
  Operation: Average Worst
  - insert: O(log(n)) O(n)
  - delete: O(log(n)) O(n)
  - remove: O(log(n)) O(n)
  - search: O(log(n)) O(n)

- Inserting elements in BST
  - BST elements must be COMPARABLE so that we can order them inside the tree.
  - When inserting an element we want to compare its value to the value stored in the current node we're considering to decide on one of the following:
    - Recurse down left subtree (< case)
    - Recurse down right subtree (> case)
    - Handle finding a duplicate value (= case)
    - Create a new node (found a null leaf)
  - Because of linear behavior (in some cases) in BST, which is very bad in terms of complexity balanced binary search trees/self balancing trees were invented.

- Removing elements in BST
  - removing elements from a BST can be seen as a two step process.
    1: Find the element we wish to remove (if it exists)
    2: Replace the node we want to remove with its successor (if any) to maintain the BST invariant.
  - Recall the BST invariant here: left subtree has smaller elements and right subtree has larger elements.

  - Find phase:
    1: we hit a null node at which point we know the value does not exist in BST
    2: comparator value equal to 0 (found it!)
    3: comparator value less than 0 (the value, if it exists, is in the left subtree)
    4: comparator value greater than 0 (the value, if it exists, is in the right subtree)

  - Remove phase: (4 cases)
    case 1: node to remove is a leaf node
    case 2: node to remove has a right subtree but no left subtree
    case 3: node to remove has a left subtree but o right subtree
    case 4: node to remove has a left subtree and a right subtree.
  - case 1 leaf node
    - If the node we wish to remove is a leaf node then we may do so without side effect.
  - case 2 and 3
    - in these cases the successor of the node we are trying to remove will be the root node of the left/right subtree.
    - It may be the case that you are removing the root node of the BST in which case it's immediate child becomes the new root as you would expect.
  - case 4
    - In which subtree will the successor of the node we are trying to remove be?: The answer is both. The successor can either be the largest value in the left subtree OR the smallest value in the right subtree.
    - A justification for why there could be more than one successor is:
      - The largest value in the left subtree satisfies BST invariant since it
        1: is larger than everything in left subtree. This follows immediately from the definition of being the largest.
        2: is smaller than everything in right subtree because it was found in the left subtree.
      - The smallest value in the right subtree satisfies BST invariant since it
        1: is smaller than everything in right subtree. This follows immediately from the definition of being the smallest.
        2: is larger than everything in left subtree because it was found in the right subtree.
    - So there are two possible successors.

- Tree Traversals (Preorder, Inorder, Postorder and Level order)
  - preorder
    - prints BEFORE the recursive calls
    - Print the value of the current node then traverse the lft subtree, followed by the right subtree.
  - inorder
    - prints BETWEEN the recursive calls
    - Traverse the left subtree, then print the value of the node and continue traversing the right subtree.
    - prints in increasing order in BST
  - postorder
    - prints AFTER the recursive calls
    - traverse the left subtree followed by the right subtree then print the value of the node
  - Level order
    - In a level order traversal we want to print the nodes as they appear one layer at a time.
    - To obtains the ordering we want to do Breadth First Search (BFS) from the root node down to the leaf nodes.
    - To do a BFS we will need to maintain a Queue of the nodes left to explore.
    - Begin with the root inside of the queue and finish when the queue is empty.
    - At each iteration we add the left child and then the right child of the current node to our queue.