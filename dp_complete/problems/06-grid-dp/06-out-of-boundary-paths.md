# Out of Boundary Paths

> Count ways to exit the grid within N moves. LC 576 · 🟡 Medium

## Problem
A ball starts at `(startRow, startColumn)` of an `m × n` grid and may move in 4 directions, at most `maxMove` times. Count the paths that move the ball **out of the grid boundary**, modulo `10^9 + 7`.

## 🧮 Math / Recurrence
`dp[k][i][j]` = ways to be at `(i,j)` using `k` moves; each step that leaves the grid counts an exit:

$$
dp[k][i][j] = \sum_{(di,dj)} dp[k-1][i-di][j-dj], \qquad
\text{answer} = \sum_{k}\ \sum_{\text{moves off-grid}} dp[k-1][\cdot][\cdot]
$$

## 🧠 Logic
The bounded move count requires a step dimension. We propagate counts layer by layer; whenever a move targets a cell outside the grid, that count contributes to the answer (one valid exit). Summing across all `maxMove` layers gives every way to leave within the limit.

## 🔢 Iteration trace (`m=2, n=2, maxMove=2, start=(0,0)`)
- 1-move exits: 2 (up, left). 2-move exits add more → total **6**.

## 🐍 Python
```python
def find_paths(m: int, n: int, max_move: int, start_row: int, start_col: int) -> int:
    MOD = 10**9 + 7
    dp = [[0] * n for _ in range(m)]
    dp[start_row][start_col] = 1
    count = 0
    for _ in range(max_move):
        nxt = [[0] * n for _ in range(m)]
        for i in range(m):
            for j in range(n):
                if dp[i][j]:
                    for di, dj in ((1, 0), (-1, 0), (0, 1), (0, -1)):
                        ni, nj = i + di, j + dj
                        if 0 <= ni < m and 0 <= nj < n:
                            nxt[ni][nj] = (nxt[ni][nj] + dp[i][j]) % MOD
                        else:
                            count = (count + dp[i][j]) % MOD
        dp = nxt
    return count


if __name__ == "__main__":
    print(find_paths(2, 2, 2, 0, 0))   # 6
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;
const long long MOD = 1e9 + 7;

int findPaths(int m, int n, int maxMove, int startRow, int startCol) {
    vector<vector<long long>> dp(m, vector<long long>(n, 0));
    dp[startRow][startCol] = 1;
    long long count = 0;
    int dirs[4][2] = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    for (int step = 0; step < maxMove; ++step) {
        vector<vector<long long>> nxt(m, vector<long long>(n, 0));
        for (int i = 0; i < m; ++i)
            for (int j = 0; j < n; ++j)
                if (dp[i][j])
                    for (auto& d : dirs) {
                        int ni = i + d[0], nj = j + d[1];
                        if (ni >= 0 && ni < m && nj >= 0 && nj < n)
                            nxt[ni][nj] = (nxt[ni][nj] + dp[i][j]) % MOD;
                        else
                            count = (count + dp[i][j]) % MOD;
                    }
        dp = nxt;
    }
    return (int)count;
}

int main() {
    cout << findPaths(2, 2, 2, 0, 0) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(maxMove·m·n)`.
- **Space:** `O(m·n)`.
