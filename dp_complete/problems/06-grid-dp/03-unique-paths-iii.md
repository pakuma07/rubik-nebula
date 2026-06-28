# Unique Paths III (Visit All)

> Must cover every empty cell → backtracking. LC 980 · 🔴 Hard

## Problem
Walk from the start cell `1` to the end cell `2`, moving in 4 directions, visiting **every** non-obstacle (`0`) cell exactly once. Count such paths.

## 🧮 Math / Recurrence
Not a table DP — exhaustive backtracking. Track the number of empty cells remaining; a path is valid only when it reaches the end with **all** empties consumed:

$$
\text{valid} \iff \text{cell} = 2 \ \wedge\ (\text{remaining empties} = 0)
$$

## 🧠 Logic
The "visit every cell exactly once" (Hamiltonian-path) constraint destroys optimal substructure, so counting via additive DP fails. Instead DFS marks a cell visited, recurses to its four neighbors, and unmarks on backtrack. Counting the empty cells up front lets us confirm full coverage when the goal is reached.

## 🔢 Iteration trace (`[[1,0,0,0],[0,0,0,0],[0,0,2,-1]]`)
- Paths covering all 9 walkable cells ending at `2` → **2**.

## 🐍 Python
```python
def unique_paths_iii(grid: list[list[int]]) -> int:
    rows, cols = len(grid), len(grid[0])
    empty = 0
    start = (0, 0)
    for i in range(rows):
        for j in range(cols):
            if grid[i][j] == 0:
                empty += 1
            elif grid[i][j] == 1:
                start = (i, j)

    def dfs(i: int, j: int, remaining: int) -> int:
        if not (0 <= i < rows and 0 <= j < cols) or grid[i][j] == -1:
            return 0
        if grid[i][j] == 2:
            return 1 if remaining == 0 else 0
        grid[i][j] = -1                       # mark visited
        total = (dfs(i + 1, j, remaining - 1) + dfs(i - 1, j, remaining - 1)
                 + dfs(i, j + 1, remaining - 1) + dfs(i, j - 1, remaining - 1))
        grid[i][j] = 0                        # unmark
        return total

    return dfs(start[0], start[1], empty + 1)


if __name__ == "__main__":
    print(unique_paths_iii([[1, 0, 0, 0], [0, 0, 0, 0], [0, 0, 2, -1]]))   # 2
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int rows, cols;

int dfs(vector<vector<int>>& g, int i, int j, int remaining) {
    if (i < 0 || i >= rows || j < 0 || j >= cols || g[i][j] == -1) return 0;
    if (g[i][j] == 2) return remaining == 0 ? 1 : 0;
    g[i][j] = -1;
    int total = dfs(g, i + 1, j, remaining - 1) + dfs(g, i - 1, j, remaining - 1)
              + dfs(g, i, j + 1, remaining - 1) + dfs(g, i, j - 1, remaining - 1);
    g[i][j] = 0;
    return total;
}

int uniquePathsIII(vector<vector<int>>& grid) {
    rows = grid.size(); cols = grid[0].size();
    int empty = 0, si = 0, sj = 0;
    for (int i = 0; i < rows; ++i)
        for (int j = 0; j < cols; ++j) {
            if (grid[i][j] == 0) ++empty;
            else if (grid[i][j] == 1) { si = i; sj = j; }
        }
    return dfs(grid, si, sj, empty + 1);
}

int main() {
    vector<vector<int>> grid = {{1, 0, 0, 0}, {0, 0, 0, 0}, {0, 0, 2, -1}};
    cout << uniquePathsIII(grid) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(4^{rc})` worst case (exhaustive).
- **Space:** `O(r·c)` recursion.
