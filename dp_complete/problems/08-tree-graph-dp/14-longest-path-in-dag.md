# Longest Path in a DAG

> Topological order + DP. Classic · 🟡 Medium

## Problem
Given a weighted directed acyclic graph, find the length of the longest path (optionally from a fixed source).

## 🧮 Math / Recurrence
Process vertices in topological order; relax outgoing edges:

$$
dist[v] = \max_{(u, v) \in E}\big(dist[u] + w(u, v)\big)
$$

## 🧠 Logic
A DAG has no cycles, so a topological order guarantees every predecessor of `v` is finalized before `v` is processed. Relaxing edges in that order yields longest paths (unlike general graphs, where longest path is NP-hard). Initialize sources to 0 (or `−∞` for unreachable nodes if a fixed source is required).

## 🔢 Iteration trace (`0→1 (5)`, `0→2 (3)`, `1→3 (6)`, `2→3 (7)`)
- Longest to 3: `0→1→3 = 11` vs `0→2→3 = 10` → **11**.

## 🐍 Python
```python
from collections import deque

def longest_path_dag(n: int, edges: list[tuple[int, int, int]]) -> int:
    graph: list[list[tuple[int, int]]] = [[] for _ in range(n)]
    indeg = [0] * n
    for u, v, w in edges:
        graph[u].append((v, w))
        indeg[v] += 1
    dist = [0] * n
    queue = deque(i for i in range(n) if indeg[i] == 0)
    while queue:
        u = queue.popleft()
        for v, w in graph[u]:
            if dist[u] + w > dist[v]:
                dist[v] = dist[u] + w
            indeg[v] -= 1
            if indeg[v] == 0:
                queue.append(v)
    return max(dist)


if __name__ == "__main__":
    print(longest_path_dag(4, [(0, 1, 5), (0, 2, 3), (1, 3, 6), (2, 3, 7)]))   # 11
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

int longestPathDAG(int n, vector<array<int, 3>>& edges) {
    vector<vector<pair<int, int>>> graph(n);
    vector<int> indeg(n, 0), dist(n, 0);
    for (auto& e : edges) { graph[e[0]].push_back({e[1], e[2]}); ++indeg[e[1]]; }
    queue<int> q;
    for (int i = 0; i < n; ++i) if (indeg[i] == 0) q.push(i);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (auto [v, w] : graph[u]) {
            dist[v] = max(dist[v], dist[u] + w);
            if (--indeg[v] == 0) q.push(v);
        }
    }
    return *max_element(dist.begin(), dist.end());
}

int main() {
    vector<array<int, 3>> edges = {{0, 1, 5}, {0, 2, 3}, {1, 3, 6}, {2, 3, 7}};
    cout << longestPathDAG(4, edges) << "\n";   // 11
}
```

## ⏱️ Complexity
- **Time:** `O(V + E)`.
- **Space:** `O(V + E)`.
