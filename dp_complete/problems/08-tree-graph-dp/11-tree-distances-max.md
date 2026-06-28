# Tree Distances I (Max Distance per Node)

> Reroot combining best two downward branches. CSES · 🔴 Hard

## Problem
For each node of a tree, find the maximum distance to any other node (the eccentricity). Output all `n` values.

## 🧮 Math / Recurrence
Down-pass: `down[v]` = farthest leaf within `v`'s subtree. Up-pass passes the best alternative from the parent:

$$
up[child] = 1 + \max\big(up[par],\ \text{best down branch of } par \text{ excluding } child\big)
$$
$$
ans[v] = \max(down[v],\ up[v])
$$

## 🧠 Logic
The farthest node from `v` lies either **inside** its subtree (`down[v]`) or **through its parent** (`up[v]`). To compute `up[child]` we need the parent's longest downward branch that does **not** go through `child`, so each node stores its top two branch lengths; the child uses the second-best when it owns the best.

## 🔢 Iteration trace (path `0-1-2`)
- `ans = [2, 1, 2]` (ends are 2 away, middle is 1).

## 🐍 Python
```python
def tree_distances(n: int, edges: list[list[int]]) -> list[int]:
    graph: list[list[int]] = [[] for _ in range(n)]
    for a, b in edges:
        graph[a].append(b)
        graph[b].append(a)
    down = [0] * n

    def dfs_down(node: int, parent: int) -> int:
        best = 0
        for nxt in graph[node]:
            if nxt != parent:
                best = max(best, 1 + dfs_down(nxt, node))
        down[node] = best
        return best

    ans = [0] * n

    def dfs_up(node: int, parent: int, up_val: int) -> None:
        ans[node] = max(down[node], up_val)
        # top two downward branches
        best1 = best2 = 0
        for nxt in graph[node]:
            if nxt != parent:
                d = 1 + down[nxt]
                if d > best1:
                    best2, best1 = best1, d
                elif d > best2:
                    best2 = d
        for nxt in graph[node]:
            if nxt != parent:
                branch = 1 + down[nxt]
                use = best1 if branch != best1 else best2
                dfs_up(nxt, node, 1 + max(up_val, use))

    dfs_down(0, -1)
    dfs_up(0, -1, 0)
    return ans


if __name__ == "__main__":
    print(tree_distances(3, [[0, 1], [1, 2]]))   # [2, 1, 2]
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int n;
vector<vector<int>> graph;
vector<int> down_, ans;

int dfsDown(int node, int parent) {
    int best = 0;
    for (int nxt : graph[node])
        if (nxt != parent) best = max(best, 1 + dfsDown(nxt, node));
    return down_[node] = best;
}

void dfsUp(int node, int parent, int upVal) {
    ans[node] = max(down_[node], upVal);
    int best1 = 0, best2 = 0;
    for (int nxt : graph[node])
        if (nxt != parent) {
            int d = 1 + down_[nxt];
            if (d > best1) { best2 = best1; best1 = d; }
            else if (d > best2) best2 = d;
        }
    for (int nxt : graph[node])
        if (nxt != parent) {
            int branch = 1 + down_[nxt];
            int use = (branch != best1) ? best1 : best2;
            dfsUp(nxt, node, 1 + max(upVal, use));
        }
}

vector<int> treeDistances(int N, vector<vector<int>>& edges) {
    n = N;
    graph.assign(n, {});
    down_.assign(n, 0);
    ans.assign(n, 0);
    for (auto& e : edges) { graph[e[0]].push_back(e[1]); graph[e[1]].push_back(e[0]); }
    dfsDown(0, -1);
    dfsUp(0, -1, 0);
    return ans;
}

int main() {
    vector<vector<int>> edges = {{0, 1}, {1, 2}};
    for (int x : treeDistances(3, edges)) cout << x << " ";   // 2 1 2
    cout << "\n";
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(n)`.
