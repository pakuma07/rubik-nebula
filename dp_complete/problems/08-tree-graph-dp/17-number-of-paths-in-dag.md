# Number of Paths in a DAG

> DP over topological order. Classic / CF · 🟡 Medium

## Problem
Count the number of distinct paths from a source `s` to a target `t` in a directed acyclic graph.

## 🧮 Math / Recurrence
Process in reverse topological order; `ways[v]` = number of paths from `v` to `t`:

$$
ways[t] = 1, \qquad ways[u] = \sum_{(u, v) \in E} ways[v]
$$

## 🧠 Logic
The number of `s→t` paths is the sum over `s`'s successors of their own path counts. Because the graph is acyclic, evaluating nodes in reverse topological order ensures every successor `v` is computed before `u`. A memoized DFS from `s` achieves the same in `O(V + E)`.

## 🔢 Iteration trace (`0→1, 0→2, 1→3, 2→3`, s=0, t=3)
- `ways[3]=1`, `ways[1]=ways[2]=1`, `ways[0]=2`. Answer = **2**.

## 🐍 Python
```python
from functools import lru_cache

def count_paths(n: int, edges: list[tuple[int, int]], s: int, t: int) -> int:
    graph: list[list[int]] = [[] for _ in range(n)]
    for u, v in edges:
        graph[u].append(v)

    @lru_cache(maxsize=None)
    def ways(u: int) -> int:
        if u == t:
            return 1
        return sum(ways(v) for v in graph[u])

    return ways(s)


if __name__ == "__main__":
    print(count_paths(4, [(0, 1), (0, 2), (1, 3), (2, 3)], 0, 3))   # 2
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int t;
vector<vector<int>> graph;
vector<long long> memo;
vector<char> done;

long long ways(int u) {
    if (u == t) return 1;
    if (done[u]) return memo[u];
    long long total = 0;
    for (int v : graph[u]) total += ways(v);
    done[u] = 1;
    return memo[u] = total;
}

long long countPaths(int n, vector<pair<int, int>>& edges, int s, int target) {
    t = target;
    graph.assign(n, {});
    memo.assign(n, 0);
    done.assign(n, 0);
    for (auto& [u, v] : edges) graph[u].push_back(v);
    return ways(s);
}

int main() {
    vector<pair<int, int>> edges = {{0, 1}, {0, 2}, {1, 3}, {2, 3}};
    cout << countPaths(4, edges, 0, 3) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(V + E)`.
- **Space:** `O(V)`.
