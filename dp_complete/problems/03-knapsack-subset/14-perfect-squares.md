# Perfect Squares (as Unbounded Knapsack)

> Coins = square numbers, minimize count. LC 279 · 🟡 Medium

## Problem
Return the fewest perfect squares summing to `n`. (Same problem as topic 02's [Perfect Squares](../02-linear-dp/36-perfect-squares.md), framed here as unbounded knapsack.)

## 🧮 Math / Recurrence
Items are the squares `1, 4, 9, …` with unlimited supply:

$$
dp[i] = 1 + \min_{k^2 \le i} dp[i - k^2], \qquad dp[0] = 0
$$

## 🧠 Logic
Building `i` means choosing a last square `k²` and adding `1` to the best for `i − k²`. Squares are reusable, so this is unbounded knapsack with a "minimize pieces" objective. The set of usable coins (`k² ≤ i`) is generated on the fly.

## 🔢 Iteration trace (`n = 13`)
- `dp[4]=1, dp[9]=1`.
- `dp[13] = min(dp[12]+1, dp[9]+1, dp[4]+1) = min(?, 2, 2) = 2` (`13 = 4 + 9`).

**Answer = 2.**

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
    print(num_squares(13))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <climits>
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
    cout << numSquares(13) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n\sqrt{n})`.
- **Space:** `O(n)`.
