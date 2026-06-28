# Egg Drop

> eggs × moves DP, or interval form. LC 887 · 🔴 Hard

## Problem
With `k` eggs and `n` floors, find the minimum number of moves that **guarantees** identifying the critical floor (above which an egg breaks), in the worst case.

## 🧮 Math / Recurrence
Flip the question: `dp[m][k]` = the maximum number of floors distinguishable with `m` moves and `k` eggs:

$$
dp[m][k] = dp[m-1][k-1] + dp[m-1][k] + 1
$$

Answer = smallest `m` with `dp[m][k] ≥ n`.

## 🧠 Logic
Each drop splits the problem: if the egg **breaks**, we have `m−1` moves and `k−1` eggs for the floors below (`dp[m-1][k-1]`); if it **survives**, `m−1` moves and `k` eggs for the floors above (`dp[m-1][k]`); plus the current floor (`+1`). This `O(nk)` formulation beats the `O(n²k)` interval recurrence.

## 🔢 Iteration trace (`k = 2, n = 6`)
- `m=3`: `dp[3][2] = 6 ≥ 6` → answer **3**.

## 🐍 Python
```python
def super_egg_drop(k: int, n: int) -> int:
    dp = [0] * (k + 1)
    moves = 0
    while dp[k] < n:
        moves += 1
        for eggs in range(k, 0, -1):
            dp[eggs] = dp[eggs] + dp[eggs - 1] + 1
    return moves


if __name__ == "__main__":
    print(super_egg_drop(2, 6))   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

int superEggDrop(int k, int n) {
    vector<int> dp(k + 1, 0);
    int moves = 0;
    while (dp[k] < n) {
        ++moves;
        for (int eggs = k; eggs >= 1; --eggs)
            dp[eggs] = dp[eggs] + dp[eggs - 1] + 1;
    }
    return moves;
}

int main() {
    cout << superEggDrop(2, 6) << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(k log n)` (moves grow logarithmically).
- **Space:** `O(k)`.
