# Where Will the Ball Fall

> Simulate each ball column by column. LC 1706 · 🟡 Medium

## Problem
A grid of diagonal boards (`1` = redirect right, `-1` = redirect left) drops a ball into each column from the top. Return, for each starting column, the bottom column it falls out of, or `-1` if it gets stuck.

## 🧮 Math / Recurrence
For each ball, advance one row at a time; the board at `grid[row][col]` deflects it, and it passes only if the neighbouring board agrees:

$$
col' = col + grid[row][col], \quad \text{stuck if } col' \notin [0, n) \ \text{or}\ grid[row][col'] \ne grid[row][col]
$$

## 🧠 Logic
A ball gets wedged in a "V" — when its board points one way but the adjacent board points back — or when it would leave the side wall. Otherwise it slides diagonally to the next row's column `col + direction`. Simulating row by row per ball is `O(m·n)` total and needs no DP table.

## 🔢 Iteration trace (single column with consistent right boards)
- A ball on all-`1` boards drifts right each row, exiting one column further right per row.

## 🐍 Python
```python
def find_ball(grid: list[list[int]]) -> list[int]:
    m, n = len(grid), len(grid[0])
    result = []
    for start in range(n):
        col = start
        for row in range(m):
            direction = grid[row][col]
            next_col = col + direction
            if next_col < 0 or next_col >= n or grid[row][next_col] != direction:
                col = -1
                break
            col = next_col
        result.append(col)
    return result


if __name__ == "__main__":
    grid = [[1, 1, 1, -1, -1],
            [1, 1, 1, -1, -1],
            [-1, -1, -1, 1, 1],
            [1, 1, 1, 1, -1],
            [-1, -1, -1, -1, -1]]
    print(find_ball(grid))   # [1, -1, -1, -1, -1]
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

vector<int> findBall(vector<vector<int>>& grid) {
    int m = grid.size(), n = grid[0].size();
    vector<int> result(n);
    for (int start = 0; start < n; ++start) {
        int col = start;
        for (int row = 0; row < m; ++row) {
            int dir = grid[row][col];
            int next = col + dir;
            if (next < 0 || next >= n || grid[row][next] != dir) { col = -1; break; }
            col = next;
        }
        result[start] = col;
    }
    return result;
}

int main() {
    vector<vector<int>> grid = {{1, 1, 1, -1, -1}, {1, 1, 1, -1, -1},
                                {-1, -1, -1, 1, 1}, {1, 1, 1, 1, -1},
                                {-1, -1, -1, -1, -1}};
    for (int x : findBall(grid)) cout << x << " ";   // 1 -1 -1 -1 -1
    cout << "\n";
}
```

## ⏱️ Complexity
- **Time:** `O(m·n)`.
- **Space:** `O(1)` extra.
