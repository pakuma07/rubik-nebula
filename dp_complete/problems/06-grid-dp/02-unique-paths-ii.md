# Unique Paths II (Obstacles)

> Obstacle cells contribute zero paths. LC 63 · 🟡 Medium

## Problem
Same right/down grid as [Unique Paths](01-unique-paths.md), but some cells contain obstacles (`1`). Count paths from top-left to bottom-right avoiding obstacles.

## 🧮 Math / Recurrence
$$
dp[i][j] =
\begin{cases}
0 & grid[i][j] = 1 \\
dp[i-1][j] + dp[i][j-1] & \text{otherwise}
\end{cases}
$$

## 🧠 Logic
An obstacle is unreachable, so its path count is `0`, and because it feeds zero into the cells below and right, it correctly blocks every path passing through it. The start cell seeds `1` only if it is not itself an obstacle.

## 🔢 Iteration trace (`grid = [[0,0,0],[0,1,0],[0,0,0]]`)
| 1 | 1 | 1 |
|---|---|---|
| 1 | **0** | 1 |
| 1 | 1 | 2 |

`dp[2][2] = ` **2**.

## 🐍 Python
```python
def unique_paths_with_obstacles(grid: list[list[int]]) -> int:
    n = len(grid[0])
    dp = [0] * n
    dp[0] = 1
    for row in grid:
        for j in range(n):
            if row[j] == 1:
                dp[j] = 0
            elif j > 0:
                dp[j] += dp[j - 1]
    return dp[-1]


if __name__ == "__main__":
    print(unique_paths_with_obstacles([[0, 0, 0], [0, 1, 0], [0, 0, 0]]))   # 2
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int uniquePathsWithObstacles(vector<vector<int>>& grid) {
    int n = grid[0].size();
    vector<long long> dp(n, 0);
    dp[0] = 1;
    for (auto& row : grid)
        for (int j = 0; j < n; ++j) {
            if (row[j] == 1) dp[j] = 0;
            else if (j > 0) dp[j] += dp[j - 1];
        }
    return dp[n - 1];
}

int main() {
    vector<vector<int>> grid = {{0, 0, 0}, {0, 1, 0}, {0, 0, 0}};
    cout << uniquePathsWithObstacles(grid) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(m·n)`.
- **Space:** `O(n)`.
