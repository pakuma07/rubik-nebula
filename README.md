# DSA Mastery — Guides & Curated Problems

A structured study repository covering core data structures and algorithms from **beginner to
advanced**, with **detailed problem explanations**, iteration traces, mathematical reasoning, and
**Mermaid diagrams**.

Every solution is provided in **both Python and C++** so the material is useful for FAANG
interviews (Python) and competitive programming (C++ / STL).

## Structure

Each topic folder contains:
- **`guide/`** — a complete concept guide (theory, complexity, patterns, pitfalls, cheat sheet).
- **`problems/`** — one file per curated problem (LeetCode + competitive sources: **CSES**,
  **Codeforces**, **AtCoder**, **SPOJ**) with step-by-step explanations, iteration tables, math,
  and diagrams.

## Topics

| Folder | Guide | Problems |
|--------|-------|----------|
| [Arrays](Arrays/guide/Arrays-Complete-Guide.md) | Memory model, prefix sums, Kadane, difference array | Two Sum, Maximum Subarray, Product Except Self, Rotate Array, **Merge Intervals**, **Next Permutation**, CSES segment tree / difference array / median |
| [Strings](Strings/README.md) | Encodings, immutability, KMP, hashing, tries; **advanced**: Z-algorithm, Aho-Corasick, Manacher, suffix array+LCP, suffix automaton, palindromic tree | Valid Anagram, Longest Palindrome, strStr/KMP, **Edit Distance**, **Longest Common Subsequence**, CSES hashing / Z / prefix function |
| [Hashing](Hashing/guide/Hashing-Complete-Guide.md) | Hash functions, collisions, load factor | Group Anagrams, Subarray Sum = K, Longest Consecutive, **Insert Delete GetRandom**, CSES distinct / k-sum / divisibility |
| [TwoPointers](TwoPointers/guide/TwoPointers-Complete-Guide.md) | Converging, fast/slow, merge | Container With Most Water, 3Sum, Trapping Rain Water, **Sort Colors (Dutch flag)**, CSES Apartments / Ferris Wheel |
| [SlidingWindow](SlidingWindow/guide/SlidingWindow-Complete-Guide.md) | Fixed/variable windows, monotonic deque | Longest Substring, Min Window Substring, Window Maximum, **Longest Repeating Char Replacement**, **Find All Anagrams**, CSES Playlist / Subarray Sums |
| [StackQueue](StackQueue/guide/StackQueue-Complete-Guide.md) | LIFO/FIFO, deque, monotonic stack | Valid Parentheses, Daily Temperatures, Queue from Stacks, Largest Rectangle, **Min Stack**, **Evaluate RPN**, CSES Nearest Smaller, monotonic min-deque |
| [LinkedLists](LinkedLists/guide/LinkedLists-Complete-Guide.md) | Node model, dummy heads, Floyd's | Reverse List, Cycle II, Merge k Lists, LRU Cache, Copy Random Pointer, Add Two Numbers, **Reorder List**, **Reverse Nodes in k-Group** |
| [BinarySearch](BinarySearch/guide/BinarySearch-Complete-Guide.md) | Templates, bounds, search-the-answer | Rotated Array, Koko Bananas, Median of Two Arrays, **Find Min in Rotated**, **Find Peak Element**, CSES Factory Machines / Array Division, Aggressive Cows |
| [Trees](Trees/README.md) | Traversals, BST, balancing, tree DP; **advanced**: LCA binary lifting, Euler tour, HLD, centroid decomposition, rerooting, DSU on tree, virtual trees | Level Order, LCA, Validate BST, **Serialize/Deserialize**, **Max Path Sum**, CSES Subordinates / Diameter / binary-lifting LCA |
| [Heaps](Heaps/guide/Heaps-Complete-Guide.md) | Array layout, heapify, priority queues | Kth Largest, Median from Stream, Top K Frequent, **Task Scheduler**, **K Closest Points**, CSES Room Allocation |
| [Graphs](Graphs/guide/Graphs-Complete-Guide.md) | BFS/DFS, topo sort, shortest paths, DSU | Number of Islands, Course Schedule, Network Delay Time, **Rotting Oranges**, **Redundant Connection (DSU)**, **Word Ladder**, CSES MST/Kruskal, Bellman-Ford, Floyd-Warshall, 0-1 BFS |

## How to Use

1. **Read the guide** for a topic to build intuition and learn the patterns.
2. **Work through the problems** in order; each file explains the *why*, not just the *how*.
3. Trace the **iteration tables** by hand to internalize the mechanics.
4. Re-implement each solution from scratch before moving on.

## Recommended Order

```
Arrays -> Strings -> Hashing -> TwoPointers -> SlidingWindow
   -> StackQueue -> LinkedLists -> BinarySearch -> Trees -> Heaps -> Graphs
```

Earlier topics build the foundations (iteration, complexity, hashing) that later ones rely on.

## Competitive Programming Additions

Beyond the LeetCode core, each folder includes hand-picked **competitive problems** (CSES,
Codeforces, AtCoder, SPOJ) covering advanced structures and techniques:

| Folder | Competitive problems added |
|--------|----------------------------|
| Arrays | CSES Dynamic Range Sum (segment tree), Range Update (difference array + Fenwick), Stick Lengths (median) |
| Strings | CSES String Matching (Rabin-Karp + Z), Finding Borders (prefix function), CF substring hashing + LCP; **9 advanced guides** — polynomial hashing, KMP/prefix function, Z-algorithm, trie, Aho-Corasick, Manacher, suffix array+LCP (Kasai), suffix automaton, palindromic tree (SPOJ DISUBSTR/LCS, LeetCode Trie/Max-XOR/Stream of Characters) |
| Hashing | CSES Distinct Numbers, Sum of Two/Three/Four Values, Subarray Divisibility (prefix mod) |
| TwoPointers | CSES Apartments, Ferris Wheel, AtCoder count-pairs-by-sum |
| SlidingWindow | CSES Playlist, Subarray Sums I (positive window), AtCoder max-sum fixed window |
| StackQueue | CSES Nearest Smaller Values, Largest Rectangle in Histogram, Sliding Window Minimum (monotonic deque) |
| LinkedLists | LRU Cache, Copy List with Random Pointer, Add Two Numbers |
| BinarySearch | CSES Factory Machines, Array Division, SPOJ Aggressive Cows (binary search on answer) |
| Trees | CSES Subordinates (subtree sizes), Tree Diameter, Company Queries II (binary-lifting LCA); **9 advanced guides** — subtree DP, diameter/centroid, LCA binary lifting, Euler tour, DSU on tree, rerooting, HLD, centroid decomposition, virtual trees (Lomsat gelral, QTREE, IOI Race, Kingdom and its Cities) |
| Heaps | Median from Data Stream (two heaps), Top K Frequent, CSES Room Allocation (sweep + heap) |
| Graphs | CSES Road Reparation (Kruskal MST + DSU), High Score (Bellman-Ford), Shortest Routes II (Floyd-Warshall), 0-1 BFS |

**Techniques covered:** segment trees, Fenwick/BIT, difference arrays, string hashing, Z-algorithm,
KMP prefix function, monotonic stack/deque, binary search on the answer, tree DP, binary lifting
(LCA), Disjoint Set Union (Kruskal MST), Bellman-Ford, Floyd-Warshall, and 0-1 BFS.

## Advanced Graph Module

For a dedicated deep dive into advanced graph algorithms, see
[graph_advanced](graph_advanced/README.md) — 15 concept guides + 30 curated problems (Python +
C++) covering:

- BFS/DFS, flood fill, connected components
- Topological sort (Kahn's + DFS), cycle detection (directed & undirected)
- Bipartite check / 2-coloring
- Shortest paths: Dijkstra, Bellman-Ford, Floyd-Warshall, 0-1 BFS
- MST: Kruskal & Prim
- Strongly connected components (Tarjan / Kosaraju) + condensation graph
- Bridges & articulation points
- Eulerian path / circuit (Hierholzer)
- Max flow (Dinic), min cut, max-flow-min-cut duality
- Bipartite matching (Kuhn / Hopcroft-Karp), König's theorem
- Min-cost max-flow
- 2-SAT (implication graph + SCC)
- Functional graphs (successor graphs, binary lifting)
- Hungarian algorithm & general matching (Blossom)

## Advanced Data Structures Module

For a dedicated deep dive into advanced data structures, see
[ds_advanced](ds_advanced/README.md) — 16 concept guides + 48 curated problems (Python + C++)
covering:

- Stack, queue, deque; monotonic stack/queue
- Prefix sums & difference arrays (1D and 2D)
- Sparse table (idempotent RMQ)
- Fenwick / BIT (point update, prefix query; 2D variant)
- Segment tree (point update, range query)
- Segment tree with lazy propagation (range update)
- Disjoint Set Union (path compression + rank; DSU with rollback)
- Heap / priority queue
- Ordered set / policy tree (`order_of_key`, balanced BST)
- Sqrt decomposition & Mo's algorithm
- Persistent data structures (persistent segment tree)
- Segment tree beats (range chmin/chmax + sum)
- Li Chao tree (segment tree of lines)
- Wavelet tree (k-th smallest, range rank)
- Link-cut tree (dynamic forest, path queries)
- Bitset optimization (constant-factor $/64$ speedup)

## Mathematics Module

For competitive-programming and interview **mathematics**, see [maths](maths/README.md) — 20
concept guides + 60 curated problems (Python + C++) covering:

- Modular arithmetic, fast/binary exponentiation, modular inverse
- GCD/LCM, extended Euclidean algorithm
- Sieve of Eratosthenes, linear sieve, smallest-prime-factor factorization
- Divisor counting/sum, number-theoretic functions
- Combinatorics: nCr mod p, factorials + inverse factorials, Pascal, Lucas' theorem
- Inclusion-exclusion
- Euler's totient, Chinese Remainder Theorem
- Catalan / Stirling numbers, derangements
- Game theory: Nim, Sprague-Grundy
- Linear algebra: Gaussian elimination, XOR basis (linear basis over GF(2))
- Probability & expectation, linearity of expectation
- FFT / NTT (polynomial multiplication), convolutions
- Möbius function / Möbius inversion
- Pollard's rho factorization, Miller-Rabin primality
- Discrete log (BSGS) & discrete sqrt (Tonelli-Shanks)
- FWHT (XOR/AND/OR convolutions)
- Berlekamp-Massey & Kitamasa (linear recurrences)
- Matroid intersection
- Generating functions
- Burnside's lemma / Pólya enumeration

## Basics Module — Core Techniques

For the foundational **problem-solving techniques**, see [basics](basics/README.md) — 18 concept
guides + 54 curated problems (Python + C++), **diagram-heavy**, covering:

- Implementation & simulation (grids, direction arrays, state machines)
- Fast I/O & careful output
- Time & space complexity analysis (Master Theorem, amortized, ops/sec budgeting)
- Bit manipulation (masks, popcount, submask/subset enumeration, SOS DP)
- Recursion & backtracking (permutations, N-Queens, pruning)
- Constructive algorithms
- Ad-hoc / observation problems
- Casework & brute force with pruning (branch & bound)
- Interactive problems (query protocol, flushing, query budgets)
- Stress testing / random test generation
- Sorting + custom comparators (strict weak ordering)
- Binary search on a sorted array (`lower_bound` / `upper_bound`)
- Binary search on the answer (monotone feasibility predicate)
- Ternary search (unimodal functions)
- Two pointers (converging & fast/slow)
- Sliding window (fixed & variable)
- Coordinate compression
- Meet in the middle

## Geometry Module — Computational Geometry

For **computational geometry**, see [geometry](geometry/README.md) — 8 concept guides + 24
curated problems (Python + C++), **diagram-heavy**, preferring exact integer arithmetic, covering:

- Vectors, dot/cross product, orientation test
- Line / segment intersection
- Polygon area (shoelace), point-in-polygon, Pick's theorem
- Convex hull (Andrew's monotone chain / Graham scan)
- Closest pair of points (divide & conquer + sweep line)
- Line sweep for geometry (intersection detection, area of unions)
- Rotating calipers (diameter, width, min-area rectangle)
- Half-plane intersection (feasible region of linear constraints)

## Dynamic Programming Module

For **dynamic programming**, see [dynamic_programming](dynamic_programming/README.md) — 18 concept
guides + 54 curated problems (Python + C++), **diagram-heavy**, covering the foundations through
the advanced optimization techniques:

- Knapsack (0/1 & unbounded)
- Coin change / counting ways
- Longest Increasing Subsequence
- LCS / edit distance
- Grid / path-counting DP
- Sequence / array DP
- Interval DP
- Bitmask DP (subset, TSP-style)
- Tree DP
- Digit DP
- Probability / expected-value DP
- Game DP (win/lose states)
- DP optimization: prefix-sum & monotonic-queue
- DP optimization: divide & conquer
- DP optimization: convex hull trick
- DP optimization: Knuth optimization
- SOS DP (sum over subsets)
- Matrix exponentiation

## Miscellaneous Module — Games, Offline, Randomization & Amortization

For **assorted advanced techniques**, see [misc](misc/README.md) — 13 concept guides + 39 curated
problems (Python + C++), **diagram-heavy**, covering:

- Nim (nim-sum / XOR theory)
- Sprague-Grundy theorem (Grundy numbers, mex, sum of games)
- Offline query processing (Mo's algorithm, offline Fenwick sweeps)
- Divide & conquer (CDQ, parallel binary search)
- Randomization (hashing, shuffles, reservoir sampling)
- Amortization & potential arguments
- Slope trick (convex piecewise-linear DP value functions)
- Aliens trick (Lagrangian optimization for exactly-k constraints)
- Implicit treap (split/merge sequence, range reverse, rotate)
- Merge sort tree (segment tree of sorted lists for range queries)
- Mo's on tree & Mo's with updates (Euler flatten, 3D Mo's)
- Stern-Brocot tree & continued fractions (best rational approximation)
- Simplex & linear programming (tableau pivoting, duality)

## Greedy Module — Greedy Algorithms

For **greedy algorithms**, see [greedy](greedy/README.md) — 3 concept guides + 9 curated problems
(Python + C++), **diagram-heavy**, with exchange-argument correctness proofs, covering:

- Exchange-argument greedy (proving local choices are globally optimal)
- Interval scheduling / activity selection (earliest-finish-time greedy)
- Sweepline (events sorted by coordinate/time)
