# Count Paths with Exactly K Moves

> Extend state with a move-count dimension. GFG · 🟡 Medium

## Problem
Count grid paths from `(0,0)` to `(m−1,n−1)` (right/down moves) that use **exactly `k`** moves. (For a pure right/down grid the answer is non-zero only when `k = m+n−2`; the generic version allows revisiting via 4 directions.)

## 🧮 Math / Recurrence
Add a step dimension `dp[k][i][j]` = ways to reach `(i,j)` in exactly `k` moves:

$$
dp[k][i][j] = \sum_{(di,dj)} dp[k-1][i-di][j-dj]
$$

## 🧠 Logic
When the number of moves is constrained, the cell position alone is not enough state — two paths to the same cell may differ in length. Adding `k` distinguishes them. Each layer `k` is built purely from layer `k−1`, so we sweep step by step. For the classic right/down grid, only `k = m+n−2` yields the binomial count.

## 🔢 Iteration trace (`m = n = 2`, right/down, `k = 2`)
- Paths of length 2 to `(1,1)`: RD and DR → **2**.

## 🐍 Python
```python
def count_paths_k_moves(m: int, n: int, k: int) -> int:
    moves = [(1, 0), (0, 1)]
    dp = [[0] * n for _ in range(m)]
    dp[0][0] = 1
    for _ in range(k):
        nxt = [[0] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if dp[i][j]:
                    for di, dj in moves:
                        ni, nj = i + di, j + dj
                        if 0 <= ni < m and 0 <= nj < n:
                            nxt[ni][nj] += dp[i][j]
        dp = nxt
    return dp[m - 1][n - 1]


if __name__ == "__main__":
    print(count_paths_k_moves(2, 2, 2))   # 2
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int countPathsKMoves(int m, int n, int k) {
    int moves[2][2] = {{1, 0}, {0, 1}};
    vector<vector<long long>> dp(m, vector<long long>(n, 0));
    dp[0][0] = 1;
    for (int step = 0; step < k; ++step) {
        vector<vector<long long>> nxt(m, vector<long long>(n, 0));
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                if (dp[i][j])
                    for (auto& mv : moves) {
                        int ni = i + mv[0], nj = j + mv[1];
                        if (ni < m && nj < n) nxt[ni][nj] += dp[i][j];
                    }
        dp = nxt;
    }
    return (int)dp[m - 1][n - 1];
}

int main() {
    cout << countPathsKMoves(2, 2, 2) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(k·m·n)`.
- **Space:** `O(m·n)`.
