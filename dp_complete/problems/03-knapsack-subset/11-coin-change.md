# Coin Change (Minimum Coins)

> Unbounded knapsack minimizing coin count. LC 322 · 🟡 Medium

## Problem
Given coin denominations `coins` (unlimited supply) and an `amount`, return the fewest coins that sum to `amount`, or `-1` if impossible.

## 🧮 Math / Recurrence
$$
dp[a] = 1 + \min_{c \le a} dp[a - c], \qquad dp[0] = 0
$$

with capacity looped **increasing** (coins reusable).

## 🧠 Logic
To make amount `a`, try every coin as the *last* one used and add `1` to the best way of making `a − c`. Greedy (largest coin first) fails for denominations like `{1,3,4}`, so the DP exhaustively considers all last-coin choices. Unbounded reuse is why the amount loop goes upward.

## 🔢 Iteration trace (`coins = [1, 3, 4]`, `amount = 6`)
| a | candidates dp[a−c]+1 | dp[a] |
|---|----------------------|-------|
| 1 | dp[0]+1 | 1 |
| 2 | dp[1]+1 | 2 |
| 3 | dp[0]+1 | 1 |
| 4 | dp[0]+1 | 1 |
| 5 | dp[4]+1, dp[2]+1, dp[1]+1 | 2 |
| 6 | dp[5]+1, dp[3]+1, dp[2]+1 | **2** |

`6 = 3+3` ⟹ **2** (greedy `4+1+1` would give 3).

## 🐍 Python
```python
def coin_change(coins: list[int], amount: int) -> int:
    INF = float("inf")
    dp = [0] + [INF] * amount
    for a in range(1, amount + 1):
        for c in coins:
            if c <= a:
                dp[a] = min(dp[a], dp[a - c] + 1)
    return -1 if dp[amount] == INF else dp[amount]


if __name__ == "__main__":
    print(coin_change([1, 3, 4], 6))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int coinChange(vector<int>& coins, int amount) {
    const int INF = 1e9;
    vector<int> dp(amount + 1, INF);
    dp[0] = 0;
    for (int a = 1; a <= amount; ++a)
        for (int c : coins)
            if (c <= a) dp[a] = min(dp[a], dp[a - c] + 1);
    return dp[amount] == INF ? -1 : dp[amount];
}

int main() {
    vector<int> coins = {1, 3, 4};
    cout << coinChange(coins, 6) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(amount · |coins|)`.
- **Space:** `O(amount)`.
