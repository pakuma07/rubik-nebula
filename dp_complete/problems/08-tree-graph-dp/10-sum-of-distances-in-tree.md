# Sum of Distances in Tree

> The flagship rerooting problem — two DFS passes. LC 834 · 🔴 Hard

## Problem
Given an undirected tree with `n` nodes, return an array where `ans[i]` is the sum of distances from node `i` to all other nodes.

## 🧮 Math / Recurrence
Two passes. Down-pass computes subtree sizes `cnt[v]` and `ans[root]`. Up-pass rerooting:

$$
ans[child] = ans[par] + (n - 2 \cdot cnt[child])
$$

## 🧠 Logic
Moving the root from `par` to an adjacent `child`: every node **inside** the child's subtree (`cnt[child]` of them) gets one step closer, and every node **outside** gets one step farther. The net change is `(n − cnt[child]) − cnt[child] = n − 2·cnt[child]`. Computing `ans[root]` once and propagating this `O(1)` update gives all answers in `O(n)`.

## 🔢 Iteration trace (star with center 0, leaves 1,2,3)
- `ans[0] = 3` (one step each), each leaf `ans = 1 + 2·1 = 5`.

## 🐍 Python
```python
def sum_of_distances(n: int, edges: list[list[int]]) -> list[int]:
    graph: list[list[int]] = [[] for _ in range(n)]
    for a, b in edges:
        graph[a].append(b)
        graph[b].append(a)
    count = [1] * n
    ans = [0] * n

    def dfs1(node: int, parent: int, depth: int) -> None:
        ans[0] += depth if node == 0 else 0
        for nxt in graph[node]:
            if nxt != parent:
                dfs1(nxt, node, depth + 1)
                count[node] += count[nxt]

    def accumulate(node: int, parent: int) -> int:
        total = 0
        for nxt in graph[node]:
            if nxt != parent:
                total += accumulate(nxt, node) + count[nxt]
        return total

    # first DFS: sizes + ans[0]
    def sizes(node: int, parent: int) -> None:
        for nxt in graph[node]:
            if nxt != parent:
                sizes(nxt, node)
                count[node] += count[nxt]

    sizes(0, -1)
    ans[0] = accumulate(0, -1)

    def reroot(node: int, parent: int) -> None:
        for nxt in graph[node]:
            if nxt != parent:
                ans[nxt] = ans[node] + n - 2 * count[nxt]
                reroot(nxt, node)

    reroot(0, -1)
    return ans


if __name__ == "__main__":
    print(sum_of_distances(4, [[0, 1], [0, 2], [0, 3]]))   # [3, 5, 5, 5]
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int n;
vector<vector<int>> graph;
vector<long long> count_, ans;

void sizes(int node, int parent) {
    for (int nxt : graph[node])
        if (nxt != parent) { sizes(nxt, node); count_[node] += count_[nxt]; }
}

long long accumulate(int node, int parent) {
    long long total = 0;
    for (int nxt : graph[node])
        if (nxt != parent) total += accumulate(nxt, node) + count_[nxt];
    return total;
}

void reroot(int node, int parent) {
    for (int nxt : graph[node])
        if (nxt != parent) {
            ans[nxt] = ans[node] + n - 2 * count_[nxt];
            reroot(nxt, node);
        }
}

vector<long long> sumOfDistances(int N, vector<vector<int>>& edges) {
    n = N;
    graph.assign(n, {});
    count_.assign(n, 1);
    ans.assign(n, 0);
    for (auto& e : edges) { graph[e[0]].push_back(e[1]); graph[e[1]].push_back(e[0]); }
    sizes(0, -1);
    ans[0] = accumulate(0, -1);
    reroot(0, -1);
    return ans;
}

int main() {
    vector<vector<int>> edges = {{0, 1}, {0, 2}, {0, 3}};
    for (long long x : sumOfDistances(4, edges)) cout << x << " ";   // 3 5 5 5
    cout << "\n";
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(n)`.
