# Tree Distances II (Sum of Distances)

> Reroot with the n − 2·count formula. CSES · 🔴 Hard

## Problem
For each node of a tree, compute the sum of distances to all other nodes. (Identical technique to [Sum of Distances in Tree](10-sum-of-distances-in-tree.md), framed as the CSES classic.)

## 🧮 Math / Recurrence
$$
ans[child] = ans[par] + (n - 2 \cdot cnt[child])
$$

where `cnt[child]` is the size of the child's subtree under the original root.

## 🧠 Logic
When rerooting across edge `(par, child)`, the `cnt[child]` nodes in the child subtree each come 1 nearer while the remaining `n − cnt[child]` move 1 farther — a net change of `n − 2·cnt[child]`. Compute `ans[root]` and subtree sizes in one DFS, then propagate the update down the tree in a second DFS.

## 🔢 Iteration trace (path `0-1-2-3`)
- `ans = [6, 4, 4, 6]`.

## 🐍 Python
```python
import sys

def tree_distances_sum(n: int, edges: list[list[int]]) -> list[int]:
    sys.setrecursionlimit(1 << 20)
    graph: list[list[int]] = [[] for _ in range(n)]
    for a, b in edges:
        graph[a].append(b)
        graph[b].append(a)
    count = [1] * n
    ans = [0] * n

    def dfs1(node: int, parent: int, depth: int) -> None:
        ans[0] += depth
        for nxt in graph[node]:
            if nxt != parent:
                dfs1(nxt, node, depth + 1)
                count[node] += count[nxt]

    def dfs2(node: int, parent: int) -> None:
        for nxt in graph[node]:
            if nxt != parent:
                ans[nxt] = ans[node] + n - 2 * count[nxt]
                dfs2(nxt, node)

    dfs1(0, -1, 0)
    dfs2(0, -1)
    return ans


if __name__ == "__main__":
    print(tree_distances_sum(4, [[0, 1], [1, 2], [2, 3]]))   # [6, 4, 4, 6]
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int n;
vector<vector<int>> graph;
vector<long long> count_, ans;

void dfs1(int node, int parent, int depth) {
    ans[0] += depth;
    for (int nxt : graph[node])
        if (nxt != parent) { dfs1(nxt, node, depth + 1); count_[node] += count_[nxt]; }
}

void dfs2(int node, int parent) {
    for (int nxt : graph[node])
        if (nxt != parent) {
            ans[nxt] = ans[node] + n - 2 * count_[nxt];
            dfs2(nxt, node);
        }
}

vector<long long> treeDistancesSum(int N, vector<vector<int>>& edges) {
    n = N;
    graph.assign(n, {});
    count_.assign(n, 1);
    ans.assign(n, 0);
    for (auto& e : edges) { graph[e[0]].push_back(e[1]); graph[e[1]].push_back(e[0]); }
    dfs1(0, -1, 0);
    dfs2(0, -1);
    return ans;
}

int main() {
    vector<vector<int>> edges = {{0, 1}, {1, 2}, {2, 3}};
    for (long long x : treeDistancesSum(4, edges)) cout << x << " ";   // 6 4 4 6
    cout << "\n";
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(n)`.
