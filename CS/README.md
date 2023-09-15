# Computer Science Cheatsheet

- [Computer Science Cheatsheet](#computer-science-cheatsheet)
  - [Data Structure](#data-structure)
    - [Heap](#heap)
    - [Hash table](#hash-table)
    - [Spanning Tree](#spanning-tree)


## Data Structure

### Heap

1. A complete tree
2. Max or min heap

### Hash table

For storing key-values. Two types of collision handling:
1. Open Addressing
    - Linear
    - Quadric
    - Second Hash
2. Separate Cashing: Implement a linked list or array

### Spanning Tree

A spanning tree is a subset of Graph G, which has all the vertices covered with minimum possible number of edges. Hence, a spanning tree does not have cycles and it cannot be disconnected.

- All possible spanning trees of graph G, have the **same** number of edges and vertices.
- The spanning tree does not have any cycle (loops).
- Removing one edge from the spanning tree will make the graph **disconnected**, i.e. the spanning tree is **minimally connected**.
- Adding one edge to the spanning tree will create a circuit or **loop**, i.e. the spanning tree is **maximally acyclic**.