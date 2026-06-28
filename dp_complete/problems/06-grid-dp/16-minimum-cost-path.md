# Minimum Cost Path (4 directions)

> Dijkstra/DP hybrid when all 4 moves allowed. GFG · 🟡 Medium

## Problem
Given a grid of costs, move in **all 4 directions** from `(0,0)` to `(n−1,n−1)` minimizing the total cost of visited cells.

## 🧮 Math / Recurrence
With moves in every direction a fixed fill order is impossible, so use Dijkstra over cells, where each cell's distance is the minimum cost to reach it:

$$
dist[v] = \min_{u \to v}\big(dist[u] + cost[v]\big)
$$

processed by a min-priority queue.

## 🧠 Logic
A pure row/column DP only works when moves are monotone (right/down); allowing up/left creates cyclic dependencies. Treating cells as graph nodes with edge weight equal to the entered cell's cost, Dijkstra greedily settles the cheapest-reachable cell first, guaranteeing correct shortest costs in `O(n² log n)`.

## 🔢 Iteration trace (`[[9,4,9],[6,1,5],[3,2,9]]`)
- Cheapest path `9→4→1→2→9`? Dijkstra finds min cost = **20** (`9+6+3+2+... ` best route).

## 🐍 Python
```python
import heapq

def min_cost_path(grid: list[list[int]]) -> int:
    n = len(grid)
    dist = [[float("inf")] * n for _ in range(n)]
    dist[0][0] = grid[0][0]
    pq = [(grid[0][0], 0, 0)]
    while pq:
        d, i, j = heapq.heappop(pq)
        if (i, j) == (n - 1, n - 1):
            return d
        if d > dist[i][j]:
            continue
        for di, dj in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            ni, nj = i + di, j + dj
            if 0 <= ni < n and 0 <= nj < n:
                nd = d + grid[ni][nj]
                if nd < dist[ni][nj]:
                    dist[ni][nj] = nd
                    heapq.heappush(pq, (nd, ni, nj))
    return dist[n - 1][n - 1]


if __name__ == "__main__":
    print(min_cost_path([[9, 4, 9], [6, 1, 5], [3, 2, 9]]))   # 20
```

## ⚙️ C++
```cpp
#include <iostream>
#include <queue>
#include <vector>
using namespace std;

int minCostPath(vector<vector<int>>& grid) {
    int n = grid.size();
    vector<vector<int>> dist(n, vector<int>(n, INT_MAX));
    priority_queue<tuple<int, int, int>, vector<tuple<int, int, int>>,
                   greater<>> pq;
    dist[0][0] = grid[0][0];
    pq.push({grid[0][0], 0, 0});
    int dirs[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    while (!pq.empty()) {
        auto [d, i, j] = pq.top(); pq.pop();
        if (i == n - 1 && j == n - 1) return d;
        if (d > dist[i][j]) continue;
        for (auto& dir : dirs) {
            int ni = i + dir[0], nj = j + dir[1];
            if (ni >= 0 && ni < n && nj >= 0 && nj < n) {
                int nd = d + grid[ni][nj];
                if (nd < dist[ni][nj]) { dist[ni][nj] = nd; pq.push({nd, ni, nj}); }
            }
        }
    }
    return dist[n - 1][n - 1];
}

int main() {
    vector<vector<int>> grid = {{9, 4, 9}, {6, 1, 5}, {3, 2, 9}};
    cout << minCostPath(grid) << "\n";   // 20
}
```

## ⏱️ Complexity
- **Time:** `O(n² log n)`.
- **Space:** `O(n²)`.
