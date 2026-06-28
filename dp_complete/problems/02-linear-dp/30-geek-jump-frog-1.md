# Geek Jump / Frog 1 (AtCoder DP A)

> Minimize cost reaching stone `i` from one or two back. AtCoder · 🟢 Easy

## Problem
A frog on stone `1` wants to reach stone `n`. Stone `i` has height `h[i]`. From `i` it may hop to `i+1` or `i+2`, paying `|h[i] − h[dest]|`. Minimize total cost.

## 🧮 Math / Recurrence
$$
dp[i] = \min\big(dp[i-1] + |h_i - h_{i-1}|,\ dp[i-2] + |h_i - h_{i-2}|\big)
$$

$$
dp[0] = 0, \qquad dp[1] = |h_1 - h_0|
$$

## 🧠 Logic
Stone `i` is reached either from `i−1` (one hop) or `i−2` (a double hop), each paying the height gap. Take the cheaper predecessor. This is the minimization twin of [Climbing Stairs](01-climbing-stairs.md); two rolling scalars give `O(1)` space.

## 🔢 Iteration trace (`h = [10, 30, 40, 20]`)
| i | from i−1 | from i−2 | dp[i] |
|---|----------|----------|-------|
| 0 | – | – | 0 |
| 1 | 0+|30−10|=20 | – | 20 |
| 2 | 20+|40−30|=30 | 0+|40−10|=30 | 30 |
| 3 | 30+|20−40|=50 | 20+|20−30|=30 | **30** |

**Answer = 30** (`0 → 1 → 3` or `0 → 2 → 3`).

## 🐍 Python
```python
def frog_min_cost(h: list[int]) -> int:
    n = len(h)
    if n == 1:
        return 0
    prev2, prev1 = 0, abs(h[1] - h[0])      # dp[0], dp[1]
    for i in range(2, n):
        cur = min(prev1 + abs(h[i] - h[i - 1]),
                  prev2 + abs(h[i] - h[i - 2]))
        prev2, prev1 = prev1, cur
    return prev1


if __name__ == "__main__":
    print(frog_min_cost([10, 30, 40, 20]))   # 30
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <cstdlib>
#include <iostream>
#include <vector>
using namespace std;

int frogMinCost(vector<int>& h) {
    int n = h.size();
    if (n == 1) return 0;
    int prev2 = 0, prev1 = abs(h[1] - h[0]);
    for (int i = 2; i < n; ++i) {
        int cur = min(prev1 + abs(h[i] - h[i - 1]),
                      prev2 + abs(h[i] - h[i - 2]));
        prev2 = prev1; prev1 = cur;
    }
    return prev1;
}

int main() {
    vector<int> h = {10, 30, 40, 20};
    cout << frogMinCost(h) << "\n";   // 30
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.

> The generalization to "hop up to `k` stones" (Frog 2) replaces the two terms with a min over the last `k`.
