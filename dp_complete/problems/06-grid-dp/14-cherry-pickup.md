# Cherry Pickup

> Go-and-return = two simultaneous paths. LC 741 · 🔴 Hard

## Problem
On an `n × n` grid (`1` = cherry, `-1` = thorn) walk top-left → bottom-right → top-left collecting cherries (picked cells become empty). Maximize cherries collected; return `0` if no valid round trip.

## 🧮 Math / Recurrence
The round trip equals **two** people walking top-left → bottom-right simultaneously. With both at step `t`, columns `c1, c2` (rows `r1 = t−c1`, `r2 = t−c2`):

$$
dp[t][c1][c2] = grid[r1][c1] + grid[r2][c2]\,[c1 \ne c2] + \max\text{(4 move combinations)}
$$

## 🧠 Logic
Reversing the return path turns it into a second forward path, so we move two agents in lockstep. They share a step counter `t`, so only the two columns are free state. A cell touched by both agents is counted **once**. Thorns make a state invalid (`-∞`). The answer is `max(0, dp[end])`.

## 🔢 Iteration trace (`[[0,1,-1],[1,0,-1],[1,1,1]]`)
- Best round trip collects **5** cherries.

## 🐍 Python
```python
from functools import lru_cache

def cherry_pickup(grid: list[list[int]]) -> int:
    n = len(grid)

    @lru_cache(maxsize=None)
    def dp(r1: int, c1: int, c2: int) -> int:
        r2 = r1 + c1 - c2
        if (r1 >= n or c1 >= n or r2 >= n or c2 >= n
                or grid[r1][c1] == -1 or grid[r2][c2] == -1):
            return float("-inf")
        if r1 == c1 == n - 1:
            return grid[r1][c1]
        cherries = grid[r1][c1]
        if c1 != c2:
            cherries += grid[r2][c2]
        cherries += max(dp(r1 + 1, c1, c2), dp(r1 + 1, c1, c2 + 1),
                        dp(r1, c1 + 1, c2), dp(r1, c1 + 1, c2 + 1))
        return cherries

    return max(0, dp(0, 0, 0))


if __name__ == "__main__":
    print(cherry_pickup([[0, 1, -1], [1, 0, -1], [1, 1, 1]]))   # 5
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int n;
vector<vector<vector<int>>> memo;

int dp(vector<vector<int>>& g, int r1, int c1, int c2) {
    int r2 = r1 + c1 - c2;
    if (r1 >= n || c1 >= n || r2 >= n || c2 >= n ||
        g[r1][c1] == -1 || g[r2][c2] == -1)
        return INT_MIN;
    if (r1 == n - 1 && c1 == n - 1) return g[r1][c1];
    int& res = memo[r1][c1][c2];
    if (res != INT_MIN) return res;
    int cherries = g[r1][c1];
    if (c1 != c2) cherries += g[r2][c2];
    int best = max({dp(g, r1 + 1, c1, c2), dp(g, r1 + 1, c1, c2 + 1),
                    dp(g, r1, c1 + 1, c2), dp(g, r1, c1 + 1, c2 + 1)});
    if (best == INT_MIN) return res = INT_MIN;
    return res = cherries + best;
}

int cherryPickup(vector<vector<int>>& grid) {
    n = grid.size();
    memo.assign(n, vector<vector<int>>(n, vector<int>(n, INT_MIN)));
    return max(0, dp(grid, 0, 0, 0));
}

int main() {
    vector<vector<int>> grid = {{0, 1, -1}, {1, 0, -1}, {1, 1, 1}};
    cout << cherryPickup(grid) << "\n";   // 5
}
```

## ⏱️ Complexity
- **Time:** `O(n³)`.
- **Space:** `O(n³)`.
