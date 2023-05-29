# AI Cheatsheet

## Table of Contents

- [AI Cheatsheet](#ai-cheatsheet)
  - [Table of Contents](#table-of-contents)
  - [Solving Problems By Searching](#solving-problems-by-searching)
    - [Informed (Heuristic) Search Strategies](#informed-heuristic-search-strategies)
      - [Keywords](#keywords)

## Solving Problems By Searching

###  Informed (Heuristic) Search Strategies

#### Keywords

- informed search
- heuristic function
- Greedy best-first search: is a form of best-first search that expands first the node with the lowest h(n) value
- A∗ search
- admissible heuristic: is one that never overestimates the cost to reach a goal.
- consistent
- Pruning: prunes away search tree nodes that are not necessary for finding an optimal solution.
- detour index: For example, road engineers know the concept of a detour index, which is Detour index
a multiplier applied to the straight-line distance to account for the typical curvature of roads.
weighted A∗ search: we weight the heuristic value more heavily. Maybe more cost path but faster solution
- Recursive best-first search (RBFS)
- backed-up value: the best f-value of its children