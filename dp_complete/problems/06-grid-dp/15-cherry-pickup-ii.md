# Cherry Pickup II (Two Robots)

> Two robots descend row by row. LC 1463 · 🔴 Hard

## Problem
Two robots start at the top corners of a grid and move down (to the same or adjacent column each step). Collect maximum cherries; a cell shared by both robots counts once.

## 🧮 Math / Recurrence
State `dp[row][c1][c2]` = max cherries from `row` onward with the robots in columns `c1, c2`:

$$
dp[row][c1][c2] = \text{cherries}(c1, c2) + \max_{\substack{n1 \in \{c1-1,c1,c1+1\} \\ n2 \in \{c2-1,c2,c2+1\}}} dp[row+1][n1][n2]
$$

where `cherries = grid[row][c1] + grid[row][c2]·[c1≠c2]`.

## 🧠 Logic
Both robots advance one row per step, so the shared row index is implicit and only the two columns vary. At each row we try all `3 × 3` column-move combinations and take the best, adding the current cherries (deduplicated when the robots coincide). This jointly optimizes both paths instead of greedily separating them.

## 🔢 Iteration trace (`[[3,1,1],[2,5,1],[1,5,5],[2,1,1]]`)
- Robots collect `3+1 | 2+5 | 5+5 | 1+1` along optimal columns → **24**.

## 🐍 Python
```python
from functools import lru_cache

def cherry_pickup(grid: list[list[int]]) -> int:
    rows, cols = len(grid), len(grid[0])

    @lru_cache(maxsize=None)
    def dp(row: int, c1: int, c2: int) -> int:
        if not (0 <= c1 < cols and 0 <= c2 < cols):
            return float("-inf")
        cherries = grid[row][c1] + (grid[row][c2] if c1 != c2 else 0)
        if row == rows - 1:
            return cherries
        best = max(dp(row + 1, c1 + d1, c2 + d2)
                   for d1 in (-1, 0, 1) for d2 in (-1, 0, 1))
        return cherries + best

    return dp(0, 0, cols - 1)


if __name__ == "__main__":
    print(cherry_pickup([[3, 1, 1], [2, 5, 1], [1, 5, 5], [2, 1, 1]]))   # 24
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int rows, cols;
vector<vector<vector<int>>> memo;

int dp(vector<vector<int>>& g, int row, int c1, int c2) {
    if (c1 < 0 || c1 >= cols || c2 < 0 || c2 >= cols) return INT_MIN;
    int& res = memo[row][c1][c2];
    if (res != INT_MIN) return res;
    int cherries = g[row][c1] + (c1 != c2 ? g[row][c2] : 0);
    if (row == rows - 1) return res = cherries;
    int best = INT_MIN;
    for (int d1 = -1; d1 <= 1; ++d1)
        for (int d2 = -1; d2 <= 1; ++d2)
            best = max(best, dp(g, row + 1, c1 + d1, c2 + d2));
    return res = cherries + best;
}

int cherryPickup(vector<vector<int>>& grid) {
    rows = grid.size(); cols = grid[0].size();
    memo.assign(rows, vector<vector<int>>(cols, vector<int>(cols, INT_MIN)));
    return dp(grid, 0, 0, cols - 1);
}

int main() {
    vector<vector<int>> grid = {{3, 1, 1}, {2, 5, 1}, {1, 5, 5}, {2, 1, 1}};
    cout << cherryPickup(grid) << "\n";   // 24
}
```

## ⏱️ Complexity
- **Time:** `O(rows·cols²·9)`.
- **Space:** `O(rows·cols²)`.
