# Unbounded Knapsack

> 0/1 knapsack but capacity loops upward. GFG · 🟡 Medium

## Problem
Items have weights `wt[]` and values `val[]`. With capacity `W` and **unlimited copies** of each item, maximize total value.

## 🧮 Math / Recurrence
$$
dp[w] = \max\big(dp[w],\ dp[w - wt_i] + val_i\big), \quad w: wt_i \to W \ (\textbf{increasing})
$$

## 🧠 Logic
The only difference from [0/1 Knapsack](01-0-1-knapsack.md) is the **loop direction**. Going upward, `dp[w − wt_i]` may already include item `i`, so taking it again is allowed — exactly the reuse semantics of an unbounded supply.

## 🔢 Iteration trace (`wt=[1,3,4]`, `val=[10,40,50]`, `W=6`)
- Best uses six length-1 items? `6×10 = 60`. Or `3+3 → 80`. Or `4+1+1 → 70`.
- Optimal: two weight-3 items → `40+40 = ` **80**.

## 🐍 Python
```python
def unbounded_knapsack(wt: list[int], val: list[int], cap: int) -> int:
    dp = [0] * (cap + 1)
    for w_i, v_i in zip(wt, val):
        for w in range(w_i, cap + 1):        # increasing → reuse
            dp[w] = max(dp[w], dp[w - w_i] + v_i)
    return dp[cap]


if __name__ == "__main__":
    print(unbounded_knapsack([1, 3, 4], [10, 40, 50], 6))   # 80
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int unboundedKnapsack(vector<int>& wt, vector<int>& val, int cap) {
    vector<int> dp(cap + 1, 0);
    for (size_t i = 0; i < wt.size(); ++i)
        for (int w = wt[i]; w <= cap; ++w)   // increasing → reuse
            dp[w] = max(dp[w], dp[w - wt[i]] + val[i]);
    return dp[cap];
}

int main() {
    vector<int> wt = {1, 3, 4}, val = {10, 40, 50};
    cout << unboundedKnapsack(wt, val, 6) << "\n";   // 80
}
```

## ⏱️ Complexity
- **Time:** `O(n·W)`.
- **Space:** `O(W)`.
