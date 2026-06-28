# Coin Combinations (CSES)

> Combinations vs permutations by loop order. CSES · 🟡 Medium

## Problem
Given coin values `coins` (unlimited) and a target `x`, count the ways to form `x`. CSES has two variants: **Coin Combinations I** (order matters → permutations) and **II** (order does not → combinations). Both mod `1e9+7`.

## 🧮 Math / Recurrence
$$
dp[a] \mathrel{+}= dp[a - c], \qquad dp[0] = 1
$$

- **Combinations (II):** coins on the **outer** loop.
- **Permutations (I):** amount on the **outer** loop.

## 🧠 Logic
Identical to [Coin Change II](12-coin-change-ii.md) / [Combination Sum IV](13-combination-sum-iv.md): the recurrence is the same, only the loop nesting decides whether `1+2` and `2+1` collapse (combinations) or count separately (permutations). Choose the nesting that matches the variant.

## 🔢 Iteration trace (`coins = [1, 2], x = 3`)
- **Combinations:** `{1,1,1}, {1,2}` ⟹ **2**.
- **Permutations:** `1+1+1, 1+2, 2+1` ⟹ **3**.

## 🐍 Python
```python
MOD = 10**9 + 7

def count_combinations(coins: list[int], x: int) -> int:
    dp = [1] + [0] * x
    for c in coins:                          # coin outer → combinations
        for a in range(c, x + 1):
            dp[a] = (dp[a] + dp[a - c]) % MOD
    return dp[x]


def count_permutations(coins: list[int], x: int) -> int:
    dp = [1] + [0] * x
    for a in range(1, x + 1):                # amount outer → permutations
        for c in coins:
            if c <= a:
                dp[a] = (dp[a] + dp[a - c]) % MOD
    return dp[x]


if __name__ == "__main__":
    print(count_combinations([1, 2], 3))   # 2
    print(count_permutations([1, 2], 3))   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;
const long long MOD = 1e9 + 7;

int countCombinations(vector<int>& coins, int x) {
    vector<long long> dp(x + 1, 0); dp[0] = 1;
    for (int c : coins)
        for (int a = c; a <= x; ++a) dp[a] = (dp[a] + dp[a - c]) % MOD;
    return (int)dp[x];
}

int countPermutations(vector<int>& coins, int x) {
    vector<long long> dp(x + 1, 0); dp[0] = 1;
    for (int a = 1; a <= x; ++a)
        for (int c : coins) if (c <= a) dp[a] = (dp[a] + dp[a - c]) % MOD;
    return (int)dp[x];
}

int main() {
    vector<int> coins = {1, 2};
    cout << countCombinations(coins, 3) << " "
         << countPermutations(coins, 3) << "\n";   // 2 3
}
```

## ⏱️ Complexity
- **Time:** `O(x · |coins|)`.
- **Space:** `O(x)`.
