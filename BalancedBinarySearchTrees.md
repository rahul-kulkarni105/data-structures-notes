## Balanced Binary Search Trees (BBSTs)
- BBST is a self- balancing binary search tree.
- This type of tree will adjust itself in order to maintain a low(logarithmic) height allowing for faster operations such as insertions and deletions.

- Complexity analysis
    Operation: Average Worst
  - insert: O(log(n)) O(log(n))
  - delete: O(log(n)) O(log(n))
  - remove: O(log(n)) O(log(n))
  - search: O(log(n)) O(log(n))

- Tree Rotations
  - The secret ingredient to most BBST algorithms is a clever usage of a tree invariant and tree rotations.
  - A tree invariant is a property/rule you impose on your tree that it must meet after every operation. To ensure that the invariant is always satisfied a series of tree rotations are normally applied.
  - Why does this work? why are you allowed to change the structure of a tree?: In the left tree we know that left nodes are supposed to be lesser than their root nodes and this remains true for the right subtree, so we didn't break the BST invariant and thus this is a valid transformation.
  - Why Tree rotation is a valid operation?: Recall that all BBST are BST so the BST invariant holds. This means that for every node n, n.left < n and n < n.right.
  - Note: The above assumes we only have unique values, otherwise we'd have to consider the case where n.left <= n and n <= n.right.
  - It does not matter what the structure of the tree looks; all we care about is that the BST invariant holds. This means we can shuffle/transform/rotate the values and nodes in the tree as we please as long as the BST invariant remains satisfied.
  - In some BBST implementations where you often need to access the parent/uncle nodes (such as RB trees), it's convenient for nodes to not only have a reference to the left and the right child nodes but also the parent node. This can complicate tree rotations because instead of updating three pointers, now you have to update six.

- Inserting elements into AVL tree
  - An AVL tree is one of many types of Balanced Binary Search Trees (BBSTs) which allow for logarithmic O(log(n)) insertion, deletion and search operations;
  - In fact, it was the first type of BBST to be discovered. Soon after, many other types of BBST's started to emerge including the 2-3 tree, the AA tree, the scapegoat tree and it's main rival the red-black tree.
  - The property which keeps an AVl tree balanced is called the Balanced factor (BF).
  - BF(node) = H(node.right) - H(node.left)
  - where H(x) is the height of node x. Recall that H(x) is calculated as the number of edges between x and the furthest leaf.
  - The invariant in the AVL which forces it to remain balanced is the requirement that that balance factor is always either -1, 0 or +1.
  - Node information to store:
    - The actual value we're storing in the node. Note: This value must be comparable so we know how to insert it.
    - A value storing this nodes' balance factor.
    - The height of this node in the tree.
    - Pointers to the left/right child nodes.
  - What if BF of node is not -1 or 0 or +1? How do we restore the AVl tree invariant?: If a node's BF is not -1, 0 or +1 then the BF of that node is +-2 which can be adjusted using tree rotations.
  Recall: BF(node) = H(node.right) - H(node.left)
  - Four cases in rotating the tree:
    1: Left right tree
    2: left left case
    3: right right case
    4: right left case

  - sudo code for inserting nodes in AVL tree
  - Public facing method: (returns trie on successful insert and false otherwise).
  function insert(value):
    if value == null:
      return false

    // Only insert unique values
    if !contains(root, value):
      root = insert(root, value)
      nodeCount = nodeCount + 1
      return true

    // value already exists in tree
    return false

  - Private recursive method:
    function insert(node, value):
      if node == null: return Node(value)

      // Invoke the comparator function in whatever language
      cmp := compare(value, node.value)
      if cmp < 0:
        node.left = insert(node.left, value)
      else:
        node.right = insert(node.right, value)

      // Update balance factor and height values
      update(node)

      // Re-balance tree
      return balance(node)

    - update method:
    function update(node):
    // variable for left/right subtree heights
    lh := -1
    rh := -1
    if node.left != null: lh = node.left.height
    if node.right != null: rh = node.right.height

    // update this nodes' height
    node.height = 1 + max(lh, rh)

    // update balance factor
    node.bf = rh - lh

    - balance method:
    function balance(node):
    // left heavy subtree.
    if node.bf == -2:
      if node.left.bf <= 0:
        return leftLeftCase(node)
      else:
        return leftRightCase(node)

    // right heavy subtree.
    if node.bf == +2:
      if node.right.bf <= 0:
        return rightRightCase(node)
      else:
        return rightLeftCase(node)
    // Node has balance factor of -1, 0 or +1
    // which we do not need to balance.
    return node

  - 4 cases
    function leftLeftCase(node):
      return rightRotation(node)

    function leftRightCase(node):
      node.left = leftRotation(node.left)
      return leftLeftCase(node)

    function rightRightCase(node):
      return leftRotation(node)

    function rightLeftCase(node):
      node.right = rightRotation(node.right)
      return rightRightCase(node)

  - AVL Rotation Method
    function rightRotate(A):
      B := A.left
      A.left = B.left
      B.right = A
      // After rotation update balance factor and height values
      update(A)
      update(B)
      return B
    - AVL tree rotations require you to call the update method. The left rotation is symmetric to above.

- Removing elements from AVL tree
  - Removing elements from BST can be seen as 2 step process
    1: FIND the element we wish to remove (if it exists)
    2: REPLACE the node we want to remove with it's successor (if any) to maintain the BST invariant.
    Recall: BST invariant is left subtree has smaller elements and right subtree has larger elements.
  - Same as BST removal with 4 cases.