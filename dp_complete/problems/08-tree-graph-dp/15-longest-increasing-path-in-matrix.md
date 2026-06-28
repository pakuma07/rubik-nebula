# Longest Increasing Path in a Matrix

> Memoized DFS over an implicit DAG. LC 329 · 🔴 Hard

## Problem
Given an integer matrix, return the length of the longest strictly increasing path (moving in 4 directions).

## 🧮 Math / Recurrence
Each cell points to strictly-larger neighbors, forming a DAG; memoize the longest chain from each cell:

$$
dp[i][j] = 1 + \max_{\substack{(x,y)\ \text{neighbor} \\ M[x][y] > M[i][j]}} dp[x][y]
$$

## 🧠 Logic
Because moves only go to strictly larger values, there are no cycles — the grid is an implicit DAG. A memoized DFS computes the longest increasing chain starting at each cell exactly once; the answer is the maximum over all cells. Memoization turns the exponential recursion into `O(m·n)`.

## 🔢 Iteration trace (`[[9,9,4],[6,6,8],[2,1,1]]`)
- Path `1→2→6→9` → length **4**.

## 🐍 Python
```python
from functools import lru_cache

def longest_increasing_path(matrix: list[list[int]]) -> int:
    if not matrix:
        return 0
    m, n = len(matrix), len(matrix[0])

    @lru_cache(maxsize=None)
    def dfs(i: int, j: int) -> int:
        best = 1
        for di, dj in ((1, 0), (-1, 0), (0, 1), (0, -1)):
            x, y = i + di, j + dj
            if 0 <= x < m and 0 <= y < n and matrix[x][y] > matrix[i][j]:
                best = max(best, 1 + dfs(x, y))
        return best

    return max(dfs(i, j) for i in range(m) for j in range(n))


if __name__ == "__main__":
    print(longest_increasing_path([[9, 9, 4], [6, 6, 8], [2, 1, 1]]))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int m, n;
vector<vector<int>> memo;

int dfs(vector<vector<int>>& mat, int i, int j) {
    if (memo[i][j]) return memo[i][j];
    int best = 1, dirs[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    for (auto& d : dirs) {
        int x = i + d[0], y = j + d[1];
        if (x >= 0 && x < m && y >= 0 && y < n && mat[x][y] > mat[i][j])
            best = max(best, 1 + dfs(mat, x, y));
    }
    return memo[i][j] = best;
}

int longestIncreasingPath(vector<vector<int>>& matrix) {
    if (matrix.empty()) return 0;
    m = matrix.size(); n = matrix[0].size();
    memo.assign(m, vector<int>(n, 0));
    int best = 0;
    for (int i = 0; i < m; ++i)
        for (int j = 0; j < n; ++j)
            best = max(best, dfs(matrix, i, j));
    return best;
}

int main() {
    vector<vector<int>> matrix = {{9, 9, 4}, {6, 6, 8}, {2, 1, 1}};
    cout << longestIncreasingPath(matrix) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(m·n)`.
- **Space:** `O(m·n)`.
