# Path With Maximum Gold

> DFS backtracking, no revisits. LC 1219 · 🟡 Medium

## Problem
In a grid where each cell holds an amount of gold (`0` = empty), start anywhere, move in 4 directions collecting gold, never visiting a cell twice and never stepping on a `0`. Return the maximum gold collectible.

## 🧮 Math / Recurrence
Exhaustive DFS from each non-zero start, marking cells:

$$
gold(i,j) = grid[i][j] + \max_{(di,dj)} gold(i+di, j+dj) \quad(\text{visited cells excluded})
$$

## 🧠 Logic
The "no revisits" constraint is path-dependent, so additive DP would double count. Instead, DFS picks up the current cell's gold, temporarily zeroes it to mark it visited, recurses to the best neighbor, then restores it on backtrack. Trying every starting cell covers all maximal collections.

## 🔢 Iteration trace (`[[0,6,0],[5,8,7],[0,9,0]]`)
- Best route `9→8→7` = **24**.

## 🐍 Python
```python
def get_maximum_gold(grid: list[list[int]]) -> int:
    rows, cols = len(grid), len(grid[0])

    def dfs(i: int, j: int) -> int:
        if not (0 <= i < rows and 0 <= j < cols) or grid[i][j] == 0:
            return 0
        gold = grid[i][j]
        grid[i][j] = 0                          # mark visited
        best = max(dfs(i + 1, j), dfs(i - 1, j),
                   dfs(i, j + 1), dfs(i, j - 1))
        grid[i][j] = gold                       # restore
        return gold + best

    return max((dfs(i, j) for i in range(rows) for j in range(cols)
                if grid[i][j]), default=0)


if __name__ == "__main__":
    print(get_maximum_gold([[0, 6, 0], [5, 8, 7], [0, 9, 0]]))   # 24
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int rows, cols;

int dfs(vector<vector<int>>& g, int i, int j) {
    if (i < 0 || i >= rows || j < 0 || j >= cols || g[i][j] == 0) return 0;
    int gold = g[i][j];
    g[i][j] = 0;
    int best = max({dfs(g, i + 1, j), dfs(g, i - 1, j),
                    dfs(g, i, j + 1), dfs(g, i, j - 1)});
    g[i][j] = gold;
    return gold + best;
}

int getMaximumGold(vector<vector<int>>& grid) {
    rows = grid.size(); cols = grid[0].size();
    int best = 0;
    for (int i = 0; i < rows; ++i)
        for (int j = 0; j < cols; ++j)
            if (grid[i][j]) best = max(best, dfs(grid, i, j));
    return best;
}

int main() {
    vector<vector<int>> grid = {{0, 6, 0}, {5, 8, 7}, {0, 9, 0}};
    cout << getMaximumGold(grid) << "\n";   // 24
}
```

## ⏱️ Complexity
- **Time:** `O(4^{cells})` worst case (bounded by gold cells).
- **Space:** `O(cells)` recursion.
