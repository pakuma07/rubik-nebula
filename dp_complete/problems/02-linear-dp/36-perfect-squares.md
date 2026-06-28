# Perfect Squares

> Unbounded coin change over square "coins". LC 279 · 🟡 Medium

## Problem
Return the fewest perfect squares (`1, 4, 9, 16, …`) that sum to `n`.

## 🧮 Math / Recurrence
$$
dp[i] = 1 + \min_{1 \le k,\ k^2 \le i} dp[i - k^2], \qquad dp[0] = 0
$$

## 🧠 Logic
This is unbounded coin change where the coins are squares: to build `i`, pick a square `k²` as the last term and add `1` to the best way of building `i − k²`. Iterate `i` upward; the answer is `dp[n]`. (Lagrange's four-square theorem guarantees the answer is `≤ 4`, enabling a faster math route.)

## 🔢 Iteration trace (`n = 12`)
| i | candidates `dp[i-k²]+1` | dp[i] |
|---|--------------------------|-------|
| 1 | dp[0]+1 | 1 |
| 4 | dp[0]+1 | 1 |
| 9 | dp[0]+1 | 1 |
| 12 | dp[11]+1, dp[8]+1, dp[3]+1 | min(?, 2+1, 3+1)= **3** |

`12 = 4+4+4` ⟹ **3**.

## 🐍 Python
```python
def num_squares(n: int) -> int:
    dp = [0] + [float("inf")] * n
    for i in range(1, n + 1):
        k = 1
        while k * k <= i:
            dp[i] = min(dp[i], dp[i - k * k] + 1)
            k += 1
    return dp[n]


if __name__ == "__main__":
    print(num_squares(12))   # 3
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int numSquares(int n) {
    vector<int> dp(n + 1, INT_MAX);
    dp[0] = 0;
    for (int i = 1; i <= n; ++i)
        for (int k = 1; k * k <= i; ++k)
            dp[i] = min(dp[i], dp[i - k * k] + 1);
    return dp[n];
}

int main() {
    cout << numSquares(12) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n\sqrt{n})`.
- **Space:** `O(n)`.
