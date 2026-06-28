# Number of Dice Rolls With Target Sum

> Bounded-count knapsack: d dice, faces 1..f. LC 1155 · 🟡 Medium

## Problem
Roll `n` dice each with faces `1..k`. Count the ways the faces sum to `target`, mod `1e9+7`.

## 🧮 Math / Recurrence
Let `dp[i][t]` = ways to reach sum `t` using `i` dice:

$$
dp[i][t] = \sum_{f=1}^{k} dp[i-1][t-f], \qquad dp[0][0] = 1
$$

## 🧠 Logic
Each die is one "item" used exactly once but contributing a value in `1..k` — a **bounded-count** knapsack. Adding die `i`, sum `t` over all face choices `f` that the previous `i−1` dice could have set up. The outer loop over dice enforces using exactly `n` of them.

## 🔢 Iteration trace (`n = 2, k = 6, target = 7`)
- With 2 dice summing to 7: `(1,6),(2,5),…,(6,1)` = **6** ways.

`dp[2][7] = 6`.

## 🐍 Python
```python
MOD = 10**9 + 7

def num_rolls_to_target(n: int, k: int, target: int) -> int:
    dp = [1] + [0] * target                  # dp[0][*]
    for _ in range(n):                       # one die per iteration
        ndp = [0] * (target + 1)
        for t in range(1, target + 1):
            for f in range(1, k + 1):
                if f <= t:
                    ndp[t] = (ndp[t] + dp[t - f]) % MOD
        dp = ndp
    return dp[target]


if __name__ == "__main__":
    print(num_rolls_to_target(2, 6, 7))   # 6
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;
const long long MOD = 1e9 + 7;

int numRollsToTarget(int n, int k, int target) {
    vector<long long> dp(target + 1, 0);
    dp[0] = 1;
    for (int d = 0; d < n; ++d) {
        vector<long long> ndp(target + 1, 0);
        for (int t = 1; t <= target; ++t)
            for (int f = 1; f <= k && f <= t; ++f)
                ndp[t] = (ndp[t] + dp[t - f]) % MOD;
        dp = ndp;
    }
    return (int)dp[target];
}

int main() {
    cout << numRollsToTarget(2, 6, 7) << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(n · target · k)`.
- **Space:** `O(target)`.
