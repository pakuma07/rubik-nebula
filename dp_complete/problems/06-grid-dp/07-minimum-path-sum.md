# Minimum Path Sum

> grid + min(up, left). LC 64 · 🟡 Medium

## Problem
Given a grid of non-negative costs, find a path from top-left to bottom-right (moving only right or down) minimizing the sum of values along the path.

## 🧮 Math / Recurrence
$$
dp[i][j] = grid[i][j] + \min\big(dp[i-1][j],\ dp[i][j-1]\big)
$$

with first row/column as prefix sums.

## 🧠 Logic
A cell is entered from above or from the left, so the cheapest way to reach it is its own cost plus the cheaper of those two incoming subpaths. The boundaries have only one feasible direction, giving running sums. Filling row by row guarantees both predecessors are ready.

## 🔢 Iteration trace (`grid = [[1,3,1],[1,5,1],[4,2,1]]`)
| 1 | 4 | 5 |
|---|---|---|
| 2 | 7 | 6 |
| 6 | 8 | 7 |

`dp[2][2] = ` **7** (path `1→3→1→1→1`).

## 🐍 Python
```python
def min_path_sum(grid: list[list[int]]) -> int:
    m, n = len(grid), len(grid[0])
    dp = [0] * n
    dp[0] = grid[0][0]
    for j in range(1, n):
        dp[j] = dp[j - 1] + grid[0][j]
    for i in range(1, m):
        dp[0] += grid[i][0]
        for j in range(1, n):
            dp[j] = grid[i][j] + min(dp[j], dp[j - 1])
    return dp[-1]


if __name__ == "__main__":
    print(min_path_sum([[1, 3, 1], [1, 5, 1], [4, 2, 1]]))   # 7
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int minPathSum(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<int> dp(n);
    dp[0] = grid[0][0];
    for (int j = 1; j < n; ++j) dp[j] = dp[j - 1] + grid[0][j];
    for (int i = 1; i < m; ++i) {
        dp[0] += grid[i][0];
        for (int j = 1; j < n; ++j)
            dp[j] = grid[i][j] + min(dp[j], dp[j - 1]);
    }
    return dp[n - 1];
}

int main() {
    vector<vector<int>> grid = {{1, 3, 1}, {1, 5, 1}, {4, 2, 1}};
    cout << minPathSum(grid) << "\n";   // 7
}
```

## ⏱️ Complexity
- **Time:** `O(m·n)`.
- **Space:** `O(n)`.
