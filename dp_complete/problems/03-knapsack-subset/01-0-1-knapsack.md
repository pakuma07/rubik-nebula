# 0/1 Knapsack

> The canonical take/skip DP with capacity looped downward. Classic / GFG · 🟡 Medium

## Problem
Given `n` items with weights `wt[]` and values `val[]` and a capacity `W`, maximize total value with each item used **at most once**.

## 🧮 Math / Recurrence
2D form:

$$
dp[i][w] = \max\big(\underbrace{dp[i-1][w]}_{\text{skip }i},\ \underbrace{dp[i-1][w-wt_i] + val_i}_{\text{take }i}\big)
$$

1D rolling form (capacity **decreasing**):

$$
dp[w] = \max\big(dp[w],\ dp[w - wt_i] + val_i\big), \quad w: W \to wt_i
$$

## 🧠 Logic
For each item there are two choices: leave it (inherit `dp[i−1][w]`) or take it if it fits (gain `val_i` plus the best packing of the remaining capacity using earlier items). Rolling to 1D, the capacity loop must go **downward** so each item is used once — an upward loop would let `dp[w−wt]` already include item `i`, double-counting it.

## 🔢 Iteration trace (`wt=[1,3,4,5]`, `val=[1,4,5,7]`, `W=7`)
Process items, capacity `7→wt` each time. Final `dp[w]`:
| w | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| dp| 0 | 1 | 1 | 4 | 5 | 7 | 8 | **9** |

`dp[7]=9` (items wt 3+4 → val 4+5).

## 🐍 Python
```python
def knapsack(wt: list[int], val: list[int], cap: int) -> int:
    dp = [0] * (cap + 1)
    for w_i, v_i in zip(wt, val):
        for w in range(cap, w_i - 1, -1):       # decreasing → 0/1
            dp[w] = max(dp[w], dp[w - w_i] + v_i)
    return dp[cap]


if __name__ == "__main__":
    print(knapsack([1, 3, 4, 5], [1, 4, 5, 7], 7))   # 9
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int knapsack(vector<int>& wt, vector<int>& val, int cap) {
    vector<int> dp(cap + 1, 0);
    for (size_t i = 0; i < wt.size(); ++i)
        for (int w = cap; w >= wt[i]; --w)       // decreasing → 0/1
            dp[w] = max(dp[w], dp[w - wt[i]] + val[i]);
    return dp[cap];
}

int main() {
    vector<int> wt = {1, 3, 4, 5}, val = {1, 4, 5, 7};
    cout << knapsack(wt, val, 7) << "\n";   // 9
}
```

## ⏱️ Complexity
- **Time:** `O(n·W)`.
- **Space:** `O(W)` with the rolling array.
