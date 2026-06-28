# Minimum Falling Path Sum II

> Track best & second-best per row to avoid same column. LC 1289 · 🔴 Hard

## Problem
Like [Minimum Falling Path Sum](09-minimum-falling-path-sum.md) but each step must move to a **different column** than the one above. Return the minimum non-zero-shift falling path sum.

## 🧮 Math / Recurrence
For row `i`, let `(m1, c1)` and `m2` be the smallest and second-smallest of the previous row's `dp`:

$$
dp[i][j] = grid[i][j] +
\begin{cases}
m1 & j \ne c1 \\
m2 & j = c1
\end{cases}
$$

## 🧠 Logic
A cell may inherit from any column except its own. The best choice is the row minimum `m1` unless that minimum sits in the same column, in which case we fall back to the second minimum `m2`. Precomputing just these two values per row makes each cell `O(1)`, reducing a naive `O(n³)` to `O(n²)`.

## 🔢 Iteration trace (`[[1,2,3],[4,5,6],[7,8,9]]`)
- Row1 min `1`@0, 2nd `2`. Row2: `[4+2,5+1,6+1]=[6,6,7]`. min `6`@0, 2nd `6`.
- Row3: `[7+6,8+6,9+6]=[13,14,15]`. Min = **13**.

## 🐍 Python
```python
def min_falling_path_sum(grid: list[list[int]]) -> int:
    n = len(grid)
    dp = grid[0][:]
    for i in range(1, n):
        # two smallest of previous row
        if dp[0] <= dp[1]:
            m1, c1, m2 = dp[0], 0, dp[1]
        else:
            m1, c1, m2 = dp[1], 1, dp[0]
        for j in range(2, n):
            if dp[j] < m1:
                m2, m1, c1 = m1, dp[j], j
            elif dp[j] < m2:
                m2 = dp[j]
        nxt = [grid[i][j] + (m1 if j != c1 else m2) for j in range(n)]
        dp = nxt
    return min(dp)


if __name__ == "__main__":
    print(min_falling_path_sum([[1, 2, 3], [4, 5, 6], [7, 8, 9]]))   # 13
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <climits>
#include <iostream>
#include <vector>
using namespace std;

int minFallingPathSum(vector<vector<int>>& grid) {
    int n = grid.size();
    vector<int> dp = grid[0];
    for (int i = 1; i < n; ++i) {
        int m1 = INT_MAX, m2 = INT_MAX, c1 = -1;
        for (int j = 0; j < n; ++j) {
            if (dp[j] < m1) { m2 = m1; m1 = dp[j]; c1 = j; }
            else if (dp[j] < m2) { m2 = dp[j]; }
        }
        vector<int> nxt(n);
        for (int j = 0; j < n; ++j)
            nxt[j] = grid[i][j] + (j != c1 ? m1 : m2);
        dp = nxt;
    }
    return *min_element(dp.begin(), dp.end());
}

int main() {
    vector<vector<int>> grid = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
    cout << minFallingPathSum(grid) << "\n";   // 13
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n)`.
