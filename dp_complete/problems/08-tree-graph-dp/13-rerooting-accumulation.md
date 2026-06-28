# Rerooting Accumulation (Generic)

> Accumulate down, redistribute the parent's complement up. CF style · 🟡 Medium

## Problem
A generic rerooting template: given a tree where each node has a weight, compute for **every** node some aggregate over the whole tree as if that node were the root (here: the weighted sum of distances, but the pattern generalizes).

## 🧮 Math / Recurrence
Two passes with a reversible merge. Down: `sub[v] = w_v + Σ sub[child]`. Up:

$$
ans[child] = f\big(ans[par],\ \text{remove } sub[child]\big)
$$

where `f` adds the contribution of "the rest of the tree" seen from `child`.

## 🧠 Logic
A single DFS only answers for the fixed root. Rerooting moves the root edge by edge: during the up-pass, a child inherits the parent's full-tree answer, *minus* its own subtree's contribution and *plus* the redistributed complement. As long as the aggregate's merge is invertible (sum, count, etc.), every node's answer is obtained in `O(n)`.

## 🔢 Iteration trace (weights `[1,1,1]`, path `0-1-2`)
- Weighted distance sums per node, e.g. `ans = [3, 2, 3]`.

## 🐍 Python
```python
def reroot_accumulate(n: int, weights: list[int], edges: list[list[int]]) -> list[int]:
    graph: list[list[int]] = [[] for _ in range(n)]
    for a, b in edges:
        graph[a].append(b)
        graph[b].append(a)
    sub = weights[:]            # subtree weight
    ans = [0] * n

    def dfs1(node: int, parent: int, depth: int) -> None:
        ans[0] += weights[node] * depth
        for nxt in graph[node]:
            if nxt != parent:
                dfs1(nxt, node, depth + 1)
                sub[node] += sub[nxt]

    total = sum(weights)

    def dfs2(node: int, parent: int) -> None:
        for nxt in graph[node]:
            if nxt != parent:
                # nodes in nxt subtree get closer, rest get farther
                ans[nxt] = ans[node] + (total - 2 * sub[nxt])
                dfs2(nxt, node)

    dfs1(0, -1, 0)
    dfs2(0, -1)
    return ans


if __name__ == "__main__":
    print(reroot_accumulate(3, [1, 1, 1], [[0, 1], [1, 2]]))   # [3, 2, 3]
```

## ⚙️ C++
```cpp
#include <iostream>
#include <numeric>
#include <vector>
using namespace std;

int n;
long long total;
vector<vector<int>> graph;
vector<long long> sub, ans;
vector<int> weights;

void dfs1(int node, int parent, int depth) {
    ans[0] += (long long)weights[node] * depth;
    for (int nxt : graph[node])
        if (nxt != parent) { dfs1(nxt, node, depth + 1); sub[node] += sub[nxt]; }
}

void dfs2(int node, int parent) {
    for (int nxt : graph[node])
        if (nxt != parent) {
            ans[nxt] = ans[node] + (total - 2 * sub[nxt]);
            dfs2(nxt, node);
        }
}

vector<long long> rerootAccumulate(int N, vector<int>& w, vector<vector<int>>& edges) {
    n = N; weights = w;
    graph.assign(n, {});
    sub.assign(w.begin(), w.end());
    ans.assign(n, 0);
    total = accumulate(w.begin(), w.end(), 0LL);
    for (auto& e : edges) { graph[e[0]].push_back(e[1]); graph[e[1]].push_back(e[0]); }
    dfs1(0, -1, 0);
    dfs2(0, -1);
    return ans;
}

int main() {
    vector<int> w = {1, 1, 1};
    vector<vector<int>> edges = {{0, 1}, {1, 2}};
    for (long long x : rerootAccumulate(3, w, edges)) cout << x << " ";   // 3 2 3
    cout << "\n";
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(n)`.
