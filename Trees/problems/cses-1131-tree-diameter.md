# Tree Diameter (CSES — Longest Path in a Tree)

| Meta | Value |
|------|-------|
| Source | CSES Problem Set — Tree Algorithms |
| Difficulty | Medium |
| Topics | Tree DFS/BFS, Diameter, Tree DP |
| Link | https://cses.fi/problemset/task/1131 |

---

## Problem Statement
Given a tree with `n` nodes, find its **diameter** — the number of edges on the **longest path**
between any two nodes.

**Example**
```
Tree:   1 - 2 - 3
                |
                4 - 5
Diameter = 4 edges (path 1-2-3-4-5).
```

---

## Two Classic Methods

### Method 1 — Two BFS/DFS Passes (unweighted or non-negative weights)
A beautiful property: from **any** node, the farthest node `u` is always an **endpoint** of some
diameter. Then the farthest node from `u` is the **other** endpoint.

```mermaid
flowchart LR
    A["pick any node s"] --> B["BFS/DFS -> farthest node u"]
    B --> C["BFS/DFS from u -> farthest node v"]
    C --> D["dist(u, v) = diameter"]
```

```python
from collections import deque

def tree_diameter_bfs(n, adj):
    def bfs(start):
        dist = [-1] * (n + 1)
        dist[start] = 0
        q = deque([start])
        far_node, far_dist = start, 0
        while q:
            x = q.popleft()
            for y in adj[x]:
                if dist[y] == -1:
                    dist[y] = dist[x] + 1
                    if dist[y] > far_dist:
                        far_dist, far_node = dist[y], y
                    q.append(y)
        return far_node, far_dist

    u, _ = bfs(1)          # farthest from arbitrary start
    v, diameter = bfs(u)   # farthest from u -> diameter
    return diameter
```

```cpp
int tree_diameter_bfs(int n, const vector<vector<int>>& adj) {
    // returns {far_node, far_dist}
    auto bfs = [&](int start) -> pair<int,int> {
        vector<int> dist(n + 1, -1);
        dist[start] = 0;
        queue<int> q;
        q.push(start);
        int far_node = start, far_dist = 0;
        while (!q.empty()) {
            int x = q.front(); q.pop();
            for (int y : adj[x]) {
                if (dist[y] == -1) {
                    dist[y] = dist[x] + 1;
                    if (dist[y] > far_dist) {
                        far_dist = dist[y];
                        far_node = y;
                    }
                    q.push(y);
                }
            }
        }
        return {far_node, far_dist};
    };

    int u = bfs(1).first;              // farthest from arbitrary start
    auto [v, diameter] = bfs(u);       // farthest from u -> diameter
    (void)v;
    return diameter;
}
```

### Method 2 — Single DFS with Tree DP (works with weights, incl. negative-free)
For each node, track the **two deepest downward branches**. A path through node `v` "turning" at
`v` has length `down1 + down2` (top two child depths). The diameter is the max over all nodes.

$$
\text{diameter} = \max_{v}\bigl(\text{deepest}_1(v) + \text{deepest}_2(v)\bigr)
$$

```python
def tree_diameter_dp(n, adj):
    best = 0
    # iterative post-order returning depth (longest downward edge-count)
    depth = [0] * (n + 1)
    visited = [False] * (n + 1)
    stack = [(1, 0, False)]
    parent = [0] * (n + 1)
    order = []
    # first compute a parent/order via DFS
    st = [1]; visited[1] = True
    while st:
        x = st.pop(); order.append(x)
        for y in adj[x]:
            if not visited[y]:
                visited[y] = True
                parent[y] = x
                st.append(y)
    for x in reversed(order):          # children processed before parents
        d1 = d2 = 0
        for y in adj[x]:
            if y != parent[x]:
                d = depth[y] + 1
                if d > d1:
                    d2, d1 = d1, d
                elif d > d2:
                    d2 = d
        depth[x] = d1
        best = max(best, d1 + d2)       # path turning at x
    return best
```

```cpp
int tree_diameter_dp(int n, const vector<vector<int>>& adj) {
    int best = 0;
    // iterative post-order returning depth (longest downward edge-count)
    vector<int> depth(n + 1, 0);
    vector<char> visited(n + 1, false);
    vector<int> parent(n + 1, 0);
    vector<int> order;
    // first compute a parent/order via DFS
    vector<int> st = {1};
    visited[1] = true;
    while (!st.empty()) {
        int x = st.back(); st.pop_back();
        order.push_back(x);
        for (int y : adj[x]) {
            if (!visited[y]) {
                visited[y] = true;
                parent[y] = x;
                st.push_back(y);
            }
        }
    }
    for (int i = (int)order.size() - 1; i >= 0; --i) {   // children before parents
        int x = order[i];
        int d1 = 0, d2 = 0;
        for (int y : adj[x]) {
            if (y != parent[x]) {
                int d = depth[y] + 1;
                if (d > d1) {
                    d2 = d1; d1 = d;
                } else if (d > d2) {
                    d2 = d;
                }
            }
        }
        depth[x] = d1;
        best = max(best, d1 + d2);       // path turning at x
    }
    return best;
}
```

---

## Trace — Method 1 on path-with-branch `1-2-3-4-5` (4 connected to 3)

Adjacency: `1-2, 2-3, 3-4, 4-5`.

**BFS from 1:** distances `{1:0, 2:1, 3:2, 4:3, 5:4}` → farthest `u = 5` (dist 4).
**BFS from 5:** distances `{5:0, 4:1, 3:2, 2:3, 1:4}` → farthest `v = 1` (dist 4).

Diameter = **4**. The first BFS lands on one true endpoint (5), the second measures across to the
other (1).

---

## Why "Farthest Node Is an Endpoint"

Suppose the farthest node `u` from start `s` were **not** a diameter endpoint. One can show the
path from `s` to `u` must intersect the diameter, and swapping arguments yields a contradiction —
`u` would have to be at least as far as a real endpoint, hence is itself a valid endpoint. This
holds for trees because there's a **unique simple path** between any two nodes (no cycles to create
shortcuts).

> ⚠️ The two-BFS trick can fail on graphs with **negative edge weights**; for plain trees with
> non-negative weights it's correct and simplest.

---

## Complexity

| Method | Time | Space |
|--------|------|-------|
| Two BFS/DFS | O(n) | O(n) |
| Single DFS tree DP | O(n) | O(n) |

---

## Related
- **Tree Distances I** (1132): farthest node **from every** node → reuse the two diameter endpoints.
- **Tree Distances II** (1133): sum of distances to all nodes → rerooting technique.

## Takeaway
Two methods, both O(n): the elegant **two-BFS** trick (farthest node is a diameter endpoint) and
the general **single-DFS tree DP** (top-two child depths at each node). The two-BFS idea relies on
the tree's unique-path property; the DP version extends cleanly to weighted trees.
