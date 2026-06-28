# Unique Paths

> Count right/down paths on a grid. LC 62 · 🟡 Medium

## Problem
A robot starts at the top-left of an `m × n` grid and may move only **right** or **down**. Count the distinct paths to the bottom-right.

## 🧮 Math / Recurrence
$$
dp[i][j] = dp[i-1][j] + dp[i][j-1], \qquad dp[0][\cdot] = dp[\cdot][0] = 1
$$

Closed form: $\binom{m+n-2}{m-1}$.

## 🧠 Logic
Every cell is reached either from the cell above or the cell to its left, so its path count is the sum of those two. The first row and column have exactly one path each (straight line). The combinatorial formula counts the arrangements of `m−1` downs among `m+n−2` total moves.

## 🔢 Iteration trace (`m = 3, n = 3`)
| | 1 | 1 | 1 |
|---|---|---|---|
| **1** | 1 | 2 | 3 |
| **1** | 3 | 6 | — |

`dp[2][2] = ` **6**.

## 🐍 Python
```python
def unique_paths(m: int, n: int) -> int:
    dp = [1] * n
    for _ in range(1, m):
        for j in range(1, n):
            dp[j] += dp[j - 1]
    return dp[-1]


if __name__ == "__main__":
    print(unique_paths(3, 3))   # 6
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int uniquePaths(int m, int n) {
    vector<int> dp(n, 1);
    for (int i = 1; i < m; ++i)
        for (int j = 1; j < n; ++j)
            dp[j] += dp[j - 1];
    return dp[n - 1];
}

int main() {
    cout << uniquePaths(3, 3) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(m·n)`.
- **Space:** `O(n)`.
