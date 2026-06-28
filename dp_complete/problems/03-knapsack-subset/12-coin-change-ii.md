# Coin Change II (Count Combinations)

> Coin outer loop ⟹ combinations, not permutations. LC 518 · 🟡 Medium

## Problem
Count the number of **combinations** of coins (unlimited supply) that sum to `amount`. Order does not matter.

## 🧮 Math / Recurrence
$$
dp[a] \mathrel{+}= dp[a - c], \qquad dp[0] = 1
$$

with **coins on the outer loop**, amount increasing inside.

## 🧠 Logic
Looping coins on the outside fixes a canonical order: each amount is built using coins `c₀, c₁, …` introduced one at a time, so `1+2` and `2+1` are counted **once**. If you swapped the loops (amount outer) you would count ordered sequences instead — that is [Combination Sum IV](13-combination-sum-iv.md).

## 🔢 Iteration trace (`coins = [1, 2, 5]`, `amount = 5`)
After coin 1: every `dp[a]=1`. After coin 2: `dp = [1,1,2,2,3,3]`. After coin 5: `dp[5] += dp[0] → 4`.

Combinations of 5: `{5}, {2,2,1}, {2,1,1,1}, {1×5}` = **4**.

## 🐍 Python
```python
def change(amount: int, coins: list[int]) -> int:
    dp = [1] + [0] * amount
    for c in coins:                          # coin outer → combinations
        for a in range(c, amount + 1):
            dp[a] += dp[a - c]
    return dp[amount]


if __name__ == "__main__":
    print(change(5, [1, 2, 5]))   # 4
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int change(int amount, vector<int>& coins) {
    vector<int> dp(amount + 1, 0);
    dp[0] = 1;
    for (int c : coins)                      // coin outer → combinations
        for (int a = c; a <= amount; ++a)
            dp[a] += dp[a - c];
    return dp[amount];
}

int main() {
    vector<int> coins = {1, 2, 5};
    cout << change(5, coins) << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(amount · |coins|)`.
- **Space:** `O(amount)`.
