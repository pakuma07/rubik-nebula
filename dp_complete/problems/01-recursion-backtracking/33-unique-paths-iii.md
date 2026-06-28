# Unique Paths III

> Walk over every empty cell exactly once. LC 980 · 🔴 Hard

## Problem
On a grid with start `1`, end `2`, empty cells `0`, and obstacles `-1`, count the paths from start to end that **walk over every empty cell exactly once** (4-directional moves).

## 🧮 Math / Recurrence
DFS tracking how many walkable cells remain; a path counts only if it ends at `2` with **all** empties consumed:

$$
\text{dfs}(r,c,remain) = \begin{cases}
[\,remain = 0\,] & board_{r,c} = 2 \\
\displaystyle\sum_{(dr,dc)} \text{dfs}(r{+}dr,\ c{+}dc,\ remain-1) & \text{else, after marking}
\end{cases}
$$

where `remain` starts at (number of `0` cells) `+ 1` (to include the end step).

## 🧠 Logic
This is Hamiltonian-path counting on the walkable cells. Mark a cell visited before recursing and unmark on return (backtracking). The crucial check at the end cell: the path is valid **only** if every empty cell has been visited, i.e. the visited count equals the total walkable count. Otherwise we'd count paths that skip cells.

## 🔢 Iteration trace
```mermaid
flowchart TD
    S["start (1), remain = #empties+1"] --> M["move to neighbor, remain--"]
    M --> E{at end (2)?}
    E -- "remain==0" --> OK["count += 1 ✅"]
    E -- "remain>0" --> NO["invalid (skipped cells)"]
    M -. dead end .-> B["unmark, backtrack"]
```

## 🐍 Python
```python
def unique_paths_iii(grid: list[list[int]]) -> int:
    rows, cols = len(grid), len(grid[0])
    empty = 0
    start = (0, 0)
    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 0:
                empty += 1
            elif grid[r][c] == 1:
                start = (r, c)

    def dfs(r: int, c: int, remain: int) -> int:
        if r < 0 or r >= rows or c < 0 or c >= cols or grid[r][c] == -1:
            return 0
        if grid[r][c] == 2:
            return 1 if remain == 0 else 0
        grid[r][c] = -1                      # mark visited
        total = (dfs(r + 1, c, remain - 1) + dfs(r - 1, c, remain - 1) +
                 dfs(r, c + 1, remain - 1) + dfs(r, c - 1, remain - 1))
        grid[r][c] = 0                       # unmark
        return total

    return dfs(start[0], start[1], empty + 1)


if __name__ == "__main__":
    grid = [[1, 0, 0, 0], [0, 0, 0, 0], [0, 0, 2, -1]]
    print(unique_paths_iii(grid))   # 2
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int rows, cols;
int dfs(vector<vector<int>>& g, int r, int c, int remain) {
    if (r < 0 || r >= rows || c < 0 || c >= cols || g[r][c] == -1) return 0;
    if (g[r][c] == 2) return remain == 0 ? 1 : 0;
    g[r][c] = -1;                            // mark
    int total = dfs(g, r + 1, c, remain - 1) + dfs(g, r - 1, c, remain - 1) +
                dfs(g, r, c + 1, remain - 1) + dfs(g, r, c - 1, remain - 1);
    g[r][c] = 0;                             // unmark
    return total;
}

int uniquePathsIII(vector<vector<int>>& grid) {
    rows = grid.size(); cols = grid[0].size();
    int empty = 0, sr = 0, sc = 0;
    for (int r = 0; r < rows; ++r)
        for (int c = 0; c < cols; ++c) {
            if (grid[r][c] == 0) ++empty;
            else if (grid[r][c] == 1) { sr = r; sc = c; }
        }
    return dfs(grid, sr, sc, empty + 1);
}

int main() {
    vector<vector<int>> g = {{1,0,0,0},{0,0,0,0},{0,0,2,-1}};
    cout << uniquePathsIII(g) << "\n";       // 2
}
```

## ⏱️ Complexity
- **Time:** `O(4^(m·n))` worst case (Hamiltonian-path search).
- **Space:** `O(m·n)` recursion depth.
