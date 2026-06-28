# Knight Probability in Chessboard

> dp board, 1/8 per move. LC 688 · 🟡 Medium

## Problem
On an `n × n` board, a knight starts at `(row, column)` and makes exactly `k` moves, each chosen uniformly among its 8 L-shaped moves. Return the probability the knight remains **on the board** after all `k` moves.

## 🧮 Math / Recurrence
`dp[i][j]` = probability of being on `(i, j)` at the current step. Each move spreads `1/8` to the 8 targets:

$$
dp'[x][y] = \sum_{(i,j) \to (x,y)} \frac{dp[i][j]}{8}
$$

Answer: sum of `dp` after `k` steps (mass that fell off the board is lost).

## 🧠 Logic
Start with all probability on the source cell. Each iteration redistributes every cell's probability equally (`/8`) to its 8 knight-moves; moves landing off-board simply vanish, so the total mass decreases by exactly the off-board probability. After `k` iterations, summing the board gives the survival probability. We forward-propagate (push) for clarity.

```mermaid
flowchart LR
    A["dp[i][j] mass"] --> B["8 knight moves"]
    B --> C["each target += dp/8"]
    C --> D["off-board moves lost"]
```

## 🔢 Iteration trace (`n=3`, `k=2`, start `(0,0)`)
- Probability ≈ **0.0625**.

## 🐍 Python
```python
def knight_probability(n: int, k: int, row: int, column: int) -> float:
    moves = [(1, 2), (2, 1), (-1, 2), (-2, 1),
             (1, -2), (2, -1), (-1, -2), (-2, -1)]
    dp = [[0.0] * n for _ in range(n)]
    dp[row][column] = 1.0
    for _ in range(k):
        ndp = [[0.0] * n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                if dp[i][j]:
                    for di, dj in moves:
                        x, y = i + di, j + dj
                        if 0 <= x < n and 0 <= y < n:
                            ndp[x][y] += dp[i][j] / 8
        dp = ndp
    return sum(map(sum, dp))


if __name__ == "__main__":
    print(round(knight_probability(3, 2, 0, 0), 4))   # 0.0625
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

double knightProbability(int n, int k, int row, int column) {
    int dr[] = {1, 2, -1, -2, 1, 2, -1, -2};
    int dc[] = {2, 1, 2, 1, -2, -1, -2, -1};
    vector<vector<double>> dp(n, vector<double>(n, 0.0));
    dp[row][column] = 1.0;
    for (int s = 0; s < k; ++s) {
        vector<vector<double>> ndp(n, vector<double>(n, 0.0));
        for (int i = 0; i < n; ++i)
            for (int j = 0; j < n; ++j)
                if (dp[i][j] > 0)
                    for (int m = 0; m < 8; ++m) {
                        int x = i + dr[m], y = j + dc[m];
                        if (x >= 0 && x < n && y >= 0 && y < n)
                            ndp[x][y] += dp[i][j] / 8;
                    }
        dp = ndp;
    }
    double total = 0;
    for (auto& r : dp) for (double v : r) total += v;
    return total;
}

int main() {
    cout << knightProbability(3, 2, 0, 0) << "\n";   // 0.0625
}
```

## ⏱️ Complexity
- **Time:** `O(k · n²)`.
- **Space:** `O(n²)`.
