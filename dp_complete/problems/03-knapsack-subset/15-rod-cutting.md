# Rod Cutting

> Unbounded knapsack on lengths maximizing price. Classic / GFG · 🟡 Medium

## Problem
A rod of length `n` can be cut into integer pieces; a piece of length `i` sells for `price[i-1]`. Maximize total revenue.

## 🧮 Math / Recurrence
Pieces of each length are unlimited:

$$
dp[L] = \max_{1 \le i \le L}\big(price[i] + dp[L - i]\big), \qquad dp[0] = 0
$$

## 🧠 Logic
For a rod of length `L`, choose the **first** piece length `i`, earn `price[i]`, and recurse on the remaining `L − i`. Because any length may be reused, capacity is looped upward (unbounded knapsack). The best over all first-piece choices is optimal.

## 🔢 Iteration trace (`price = [1,5,8,9]` for lengths 1..4, `n = 4`)
| L | best cut | dp[L] |
|---|----------|-------|
| 1 | 1 | 1 |
| 2 | 2 (price 5) | 5 |
| 3 | 3 (price 8) | 8 |
| 4 | 2+2 (5+5) | **10** |

**Answer = 10** (two length-2 pieces).

## 🐍 Python
```python
def rod_cutting(price: list[int], n: int) -> int:
    dp = [0] * (n + 1)
    for L in range(1, n + 1):
        for i in range(1, L + 1):
            dp[L] = max(dp[L], price[i - 1] + dp[L - i])
    return dp[n]


if __name__ == "__main__":
    print(rod_cutting([1, 5, 8, 9], 4))   # 10
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int rodCutting(vector<int>& price, int n) {
    vector<int> dp(n + 1, 0);
    for (int L = 1; L <= n; ++L)
        for (int i = 1; i <= L; ++i)
            dp[L] = max(dp[L], price[i - 1] + dp[L - i]);
    return dp[n];
}

int main() {
    vector<int> price = {1, 5, 8, 9};
    cout << rodCutting(price, 4) << "\n";   // 10
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n)`.
