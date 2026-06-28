# Maximal Square

> dp = min(top, left, diag) + 1. LC 221 · 🟡 Medium

## Problem
Given a binary matrix, find the area of the largest square containing only `1`s.

## 🧮 Math / Recurrence
`dp[i][j]` = side length of the largest all-ones square whose **bottom-right corner** is `(i,j)`:

$$
dp[i][j] =
\begin{cases}
0 & grid[i][j] = 0 \\
1 + \min\big(dp[i-1][j],\ dp[i][j-1],\ dp[i-1][j-1]\big) & grid[i][j] = 1
\end{cases}
$$

Answer = `max(dp)²`.

## 🧠 Logic
A square of side `k` ending at `(i,j)` requires squares of side `k−1` ending at the cell above, the cell left, and the diagonal cell — all three must be present, so the limiting one (the minimum) determines how large this corner can grow. Track the global max side and square it.

## 🔢 Iteration trace (`1`s forming a 2×2 block)
- The bottom-right of a 2×2 block gets `dp = 1 + min(1,1,1) = 2` → area **4**.

## 🐍 Python
```python
def maximal_square(matrix: list[list[str]]) -> int:
    if not matrix:
        return 0
    m, n = len(matrix), len(matrix[0])
    dp = [0] * (n + 1)
    best = 0
    for i in range(1, m + 1):
        prev = 0                                # dp[i-1][j-1]
        for j in range(1, n + 1):
            temp = dp[j]
            if matrix[i - 1][j - 1] == "1":
                dp[j] = 1 + min(dp[j], dp[j - 1], prev)
                best = max(best, dp[j])
            else:
                dp[j] = 0
            prev = temp
    return best * best


if __name__ == "__main__":
    grid = [["1", "0", "1", "0", "0"],
            ["1", "0", "1", "1", "1"],
            ["1", "1", "1", "1", "1"],
            ["1", "0", "0", "1", "0"]]
    print(maximal_square(grid))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maximalSquare(vector<vector<char>>& matrix) {
    if (matrix.empty()) return 0;
    int m = matrix.size(), n = matrix[0].size(), best = 0;
    vector<int> dp(n + 1, 0);
    for (int i = 1; i <= m; ++i) {
        int prev = 0;
        for (int j = 1; j <= n; ++j) {
            int temp = dp[j];
            if (matrix[i - 1][j - 1] == '1') {
                dp[j] = 1 + min({dp[j], dp[j - 1], prev});
                best = max(best, dp[j]);
            } else dp[j] = 0;
            prev = temp;
        }
    }
    return best * best;
}

int main() {
    vector<vector<char>> grid = {{'1', '0', '1', '0', '0'},
                                 {'1', '0', '1', '1', '1'},
                                 {'1', '1', '1', '1', '1'},
                                 {'1', '0', '0', '1', '0'}};
    cout << maximalSquare(grid) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(m·n)`.
- **Space:** `O(n)`.
