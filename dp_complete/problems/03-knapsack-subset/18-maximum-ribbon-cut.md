# Maximum Ribbon Cut

> Unbounded knapsack maximizing the *number* of pieces. GFG · 🟡 Medium

## Problem
Given a ribbon of length `n` and allowed piece lengths `lengths[]` (unlimited), cut it into the **maximum number of pieces**, using the whole ribbon. Return `-1` if impossible.

## 🧮 Math / Recurrence
$$
dp[L] = \max_{\ell \in lengths,\ \ell \le L}\big(dp[L - \ell] + 1\big), \qquad dp[0] = 0
$$

with `dp[L] = -\infty` when `L` is unreachable.

## 🧠 Logic
Like [Coin Change](11-coin-change.md) but **maximizing** count instead of minimizing. A piece of length `ℓ` contributes `+1` and leaves `L − ℓ` to cut. The whole ribbon must be consumed, so unreachable lengths stay `−∞` and propagate.

## 🔢 Iteration trace (`n = 7`, `lengths = [2, 3, 5]`)
| L | best | dp[L] |
|---|------|-------|
| 2 | dp[0]+1 | 1 |
| 3 | dp[0]+1 | 1 |
| 4 | dp[2]+1 | 2 |
| 5 | dp[2]+1, dp[0]+1 | 2 |
| 6 | dp[4]+1, dp[3]+1 | 3 |
| 7 | dp[5]+1, dp[4]+1, dp[2]+1 | **3** |

`7 = 2+2+3` ⟹ **3** pieces.

## 🐍 Python
```python
def max_ribbon_cut(n: int, lengths: list[int]) -> int:
    NEG = float("-inf")
    dp = [0] + [NEG] * n
    for L in range(1, n + 1):
        for ell in lengths:
            if ell <= L and dp[L - ell] != NEG:
                dp[L] = max(dp[L], dp[L - ell] + 1)
    return -1 if dp[n] == NEG else dp[n]


if __name__ == "__main__":
    print(max_ribbon_cut(7, [2, 3, 5]))   # 3
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int maxRibbonCut(int n, vector<int>& lengths) {
    const int NEG = -1e9;
    vector<int> dp(n + 1, NEG);
    dp[0] = 0;
    for (int L = 1; L <= n; ++L)
        for (int ell : lengths)
            if (ell <= L && dp[L - ell] != NEG)
                dp[L] = max(dp[L], dp[L - ell] + 1);
    return dp[n] == NEG ? -1 : dp[n];
}

int main() {
    vector<int> lengths = {2, 3, 5};
    cout << maxRibbonCut(7, lengths) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n · |lengths|)`.
- **Space:** `O(n)`.
