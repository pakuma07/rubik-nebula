# Number of Paths with Target Sum (Grid)

> Add a running-sum dimension. Variant · 🟡 Medium

## Problem
Moving right/down on a grid of values, count the paths from top-left to bottom-right whose collected values sum to a target `S`.

## 🧮 Math / Recurrence
State `dp[i][j][s]` = paths reaching `(i,j)` with accumulated sum `s`:

$$
dp[i][j][s] = dp[i-1][j][\,s - g_{ij}] + dp[i][j-1][\,s - g_{ij}]
$$

## 🧠 Logic
Position alone can't tell paths apart by their sums, so we extend the state with the running total. Each cell adds its own value, so the contributing predecessors are queried at sum `s − grid[i][j]`. A hashmap per cell keeps only the reachable sums, which is more memory-efficient than a dense third axis.

## 🔢 Iteration trace (`grid = [[1,2],[1,1]]`, `S = 3`)
- Path `1→2→1 = 4` ✗; `1→1→1 = 3` ✓. Count = **1**.

## 🐍 Python
```python
from collections import defaultdict

def count_paths_target_sum(grid: list[list[int]], target: int) -> int:
    m, n = len(grid), len(grid[0])
    dp = [[defaultdict(int) for _ in range(n)] for _ in range(m)]
    dp[0][0][grid[0][0]] = 1
    for i in range(m):
        for j in range(n):
            if i == 0 and j == 0:
                continue
            for src_i, src_j in ((i - 1, j), (i, j - 1)):
                if 0 <= src_i < m and 0 <= src_j < n:
                    for s, c in dp[src_i][src_j].items():
                        dp[i][j][s + grid[i][j]] += c
    return dp[m - 1][n - 1].get(target, 0)


if __name__ == "__main__":
    print(count_paths_target_sum([[1, 2], [1, 1]], 3))   # 1
```

## ⚙️ C++
```cpp
#include <iostream>
#include <unordered_map>
#include <vector>
using namespace std;

int countPathsTargetSum(vector<vector<int>>& grid, int target) {
    int m = grid.size(), n = grid[0].size();
    vector<vector<unordered_map<int, long long>>> dp(
        m, vector<unordered_map<int, long long>>(n));
    dp[0][0][grid[0][0]] = 1;
    for (int i = 0; i < m; ++i)
        for (int j = 0; j < n; ++j) {
            if (i == 0 && j == 0) continue;
            int srcs[2][2] = {{i - 1, j}, {i, j - 1}};
            for (auto& s : srcs) {
                int si = s[0], sj = s[1];
                if (si >= 0 && sj >= 0)
                    for (auto& [sum, c] : dp[si][sj])
                        dp[i][j][sum + grid[i][j]] += c;
            }
        }
    auto& last = dp[m - 1][n - 1];
    auto it = last.find(target);
    return it == last.end() ? 0 : (int)it->second;
}

int main() {
    vector<vector<int>> grid = {{1, 2}, {1, 1}};
    cout << countPathsTargetSum(grid, 3) << "\n";   // 1
}
```

## ⏱️ Complexity
- **Time:** `O(m·n·S)` (distinct sums).
- **Space:** `O(m·n·S)`.
