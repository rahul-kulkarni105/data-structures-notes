## Union Fnd (Disjoint Set)
- Union Find is a data structure that keeps track of elements which are split into one or more disjoint sets.
- It has two primary operations:
  - find: given an element it will find will tell you what group the element belongs to
  - union: merges two groups together.

- Uses
  - Kruskal's minimum spanning tree algorithm
  - Grid percolation
  - Network connectivity
  - Latest common ancestor in trees
  - Image processing

- Complexity analysis
  > alpha(n): amortized constant time, (almost constant time)
  - construction: O(n)
  - union: alpha(n)
  - find: alpha(n)
  - get component size: alpha(n)
  - check if connected: alpha(n)
  - count components: O(1)

- Kruskal's minimum spanning tree algorithm
  - given a graph G = (V, E) we want to find a Minimum SPanning Tree in the graph (it may not be unique).
  - A minimum spanning tree is a subset of the edges which connect all vertices in the graph with the minimal total edge cost.
    1: sort edges by ascending edge weight
    2: walk through the sorted edges and look at the two nodes the edge belongs to, if the nodes are already unified we don't include this edge, otherwise we include it and unify the nodes.
    3: the algorithm terminates when every edge has been processed or all the vertices have been unified.

- Operations
  - To begin using union find, first construct a BIJECTION (a mapping) between your objects and the integers in the range [0, n.
  Note: This step is not necessary in general, but it will allow us to construct an array-based union find.
  - Find operation:
    - To find which component a particular element belongs to find the root of that component by following the parent nodes until a self loop is reached (a node who's parent is itself)
  - Union Operation:
    - To unify two elements find which are the root nodes of each component and if the root nodes are different make one of the root nodes be the parent of the other.
  - In this ds, we don't "un-union" elements. In general, this would be very inefficient to do since we would have to update all the children of a node.
  - The number of components is equal to the number of roots remaining. lso, remark that the number of root nodes never increases.

- Path compression Union find
  - This is how union find becomes one of the most efficient ds out there.