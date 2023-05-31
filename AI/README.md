# AI Cheatsheet

## Table of Contents

- [AI Cheatsheet](#ai-cheatsheet)
  - [Table of Contents](#table-of-contents)
  - [Solving Problems By Searching](#solving-problems-by-searching)
    - [Informed (Heuristic) Search Strategies](#informed-heuristic-search-strategies)
      - [Keywords](#keywords)
    - [Constraints Satisfaction Problems (CSP)](#constraints-satisfaction-problems-csp)
      - [Keywords](#keywords-1)

## Solving Problems By Searching

###  Informed (Heuristic) Search Strategies

#### Keywords

- **Informed search**
- **Heuristic function**
- **Greedy best-first search** is a form of best-first search that expands first the node with the lowest h(n) value
- **A∗ search**
- **Admissible heuristic** is one that never overestimates the cost to reach a goal.
- **Consistent**
- **Pruning:** prunes away search tree nodes that are not necessary for finding an optimal solution.
- **Detour index:** For example, road engineers know the concept of a detour index, which is Detour index
a multiplier applied to the straight-line distance to account for the typical curvature of roads.
- **Weighted A∗ search:** we weight the heuristic value more heavily. Maybe more cost path but faster solution
- **Recursive best-first search (RBFS)**
- **Backed-up value:** the best f-value of its children

### Constraints Satisfaction Problems (CSP)

#### Keywords

- **Complete consistent** assignment is one in which every variable is assigned a value
- **Partial assignment** is one that leaves some variables Solution
unassigned
- **Partial solution** is a partial assignment that is consistent.
- **Constraint graph**
- **Unary constraint** restricts the value of a single variable
- **Binary constraint** relates two variables
- *Global constraint:** One of the most common global constraints is *Alldiff*