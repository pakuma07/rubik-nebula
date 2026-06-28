# Stone Game

> Same score-diff DP; first player always wins. LC 877 · 🟡 Medium

## Problem
Even number of piles in a row; Alice and Bob alternately take a whole pile from either end. Both play optimally and the total is odd. Return `true` if Alice wins.

## 🧮 Math / Recurrence
Identical score-difference minimax to Predict the Winner:

$$
dp[i][j] = \max\big(piles[i] - dp[i+1][j],\ piles[j] - dp[i][j-1]\big)
$$

Alice wins iff `dp[0][n-1] > 0`. (Mathematically, with even `n` and odd total, the answer is always `true`.)

## 🧠 Logic
The DP is general, but a parity argument makes the result a constant: with an even number of piles, Alice can decide upfront to take **all even-indexed** or **all odd-indexed** piles (the two groups are disjoint and she controls which by her first move). Since the total is odd, one group sums higher, so Alice always wins. We still show the DP for completeness.

## 🔢 Iteration trace (`[5,3,4,5]`)
- Alice takes 5 (index 0), forces remaining → diff > 0 → **True**.

## 🐍 Python
```python
def stone_game(piles: list[int]) -> bool:
    n = len(piles)
    dp = piles[:]
    for i in range(n - 1, -1, -1):
        nxt = dp[:]
        for j in range(i + 1, n):
            nxt[j] = max(piles[i] - nxt[j], piles[j] - dp[j - 1])
        dp = nxt
    return dp[n - 1] > 0


if __name__ == "__main__":
    print(stone_game([5, 3, 4, 5]))   # True
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

bool stoneGame(vector<int>& piles) {
    int n = piles.size();
    vector<int> dp(piles);
    for (int i = n - 1; i >= 0; --i) {
        vector<int> nxt = dp;
        for (int j = i + 1; j < n; ++j)
            nxt[j] = max(piles[i] - nxt[j], piles[j] - dp[j - 1]);
        dp = nxt;
    }
    return dp[n - 1] > 0;
}

int main() {
    vector<int> piles = {5, 3, 4, 5};
    cout << boolalpha << stoneGame(piles) << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(n²)` (or `O(1)` via the parity argument).
- **Space:** `O(n)`.
