# Critical Connections (Bridges)

> DFS low-link values. LC 1192 · 🔴 Hard

## Problem
In an undirected connected network, find all **bridges** — edges whose removal disconnects the graph.

## 🧮 Math / Recurrence
Tarjan's bridge-finding: assign each node a DFS discovery time `disc[u]` and a `low[u]`:

$$
low[u] = \min\big(disc[u],\ \min_{(u,v)} low[v],\ \min_{(u,w)\ \text{back edge}} disc[w]\big)
$$

Edge `(u, v)` is a bridge iff `low[v] > disc[u]`.

## 🧠 Logic
`low[v]` is the earliest node reachable from `v`'s subtree using at most one back edge. If `low[v] > disc[u]`, then `v`'s subtree has **no** alternative route back to `u` or earlier, so the only connection is the tree edge `(u, v)` — a bridge. (This is graph DFS rather than tabular DP, but shares the post-order aggregation pattern.)

## 🔢 Iteration trace (`n=4`, edges `[[0,1],[1,2],[2,0],[1,3]]`)
- The triangle `0-1-2` has no bridge; edge `1-3` is the only bridge → `[[1,3]]`.

## 🐍 Python
```python
def critical_connections(n: int, connections: list[list[int]]) -> list[list[int]]:
    graph: list[list[int]] = [[] for _ in range(n)]
    for a, b in connections:
        graph[a].append(b)
        graph[b].append(a)
    disc = [-1] * n
    low = [0] * n
    bridges: list[list[int]] = []
    timer = 0

    def dfs(u: int, parent: int) -> None:
        nonlocal timer
        disc[u] = low[u] = timer
        timer += 1
        for v in graph[u]:
            if v == parent:
                continue
            if disc[v] == -1:
                dfs(v, u)
                low[u] = min(low[u], low[v])
                if low[v] > disc[u]:
                    bridges.append([u, v])
            else:
                low[u] = min(low[u], disc[v])

    dfs(0, -1)
    return bridges


if __name__ == "__main__":
    print(critical_connections(4, [[0, 1], [1, 2], [2, 0], [1, 3]]))   # [[1, 3]]
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

vector<vector<int>> graph, bridges;
vector<int> disc, low;
int timer_;

void dfs(int u, int parent) {
    disc[u] = low[u] = timer_++;
    for (int v : graph[u]) {
        if (v == parent) continue;
        if (disc[v] == -1) {
            dfs(v, u);
            low[u] = min(low[u], low[v]);
            if (low[v] > disc[u]) bridges.push_back({u, v});
        } else {
            low[u] = min(low[u], disc[v]);
        }
    }
}

vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
    graph.assign(n, {});
    disc.assign(n, -1);
    low.assign(n, 0);
    bridges.clear();
    timer_ = 0;
    for (auto& c : connections) { graph[c[0]].push_back(c[1]); graph[c[1]].push_back(c[0]); }
    dfs(0, -1);
    return bridges;
}

int main() {
    vector<vector<int>> conn = {{0, 1}, {1, 2}, {2, 0}, {1, 3}};
    for (auto& b : criticalConnections(4, conn))
        cout << b[0] << "-" << b[1] << "\n";   // 1-3
}
```

## ⏱️ Complexity
- **Time:** `O(V + E)`.
- **Space:** `O(V + E)`.
