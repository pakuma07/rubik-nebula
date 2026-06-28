# Domino and Tromino Tiling

> A tiling recurrence with a 3-step memory. LC 790 · 🟡 Medium

## Problem
Tile a `2 × n` board with `2×1` dominoes and L-shaped trominoes (rotations allowed). Count the tilings mod `1e9+7`.

## 🧮 Math / Recurrence
$$
dp[n] = 2\,dp[n-1] + dp[n-3], \qquad dp[0]=1,\ dp[1]=1,\ dp[2]=2
$$

## 🧠 Logic
Classify a tiling by how its **last column** is completed. Careful case analysis (full columns vs the jagged edge left by a tromino) collapses to: each new fully-filled width comes from `dp[n−1]` in two symmetric ways, plus a tromino-pair configuration reaching back to `dp[n−3]`. Three rolling values track the recurrence.

## 🔢 Iteration trace
| n | 0 | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|---|
| dp| 1 | 1 | 2 | 5 | 11 | 24 |

`dp[3]=2·2+1=5`, `dp[4]=2·5+1=11`, `dp[5]=2·11+2=24`.

## 🐍 Python
```python
MOD = 10**9 + 7

def num_tilings(n: int) -> int:
    if n <= 2:
        return [1, 1, 2][n]
    a, b, c = 1, 1, 2           # dp[0], dp[1], dp[2]
    for _ in range(3, n + 1):
        a, b, c = b, c, (2 * c + a) % MOD
    return c


if __name__ == "__main__":
    print(num_tilings(5))   # 24
```

## ⚙️ C++
```cpp
#include <iostream>
using namespace std;
const long long MOD = 1e9 + 7;

int numTilings(int n) {
    long long dp[3] = {1, 1, 2};
    if (n <= 2) return dp[n];
    long long a = 1, b = 1, c = 2;
    for (int i = 3; i <= n; ++i) {
        long long next = (2 * c + a) % MOD;
        a = b; b = c; c = next;
    }
    return (int)c;
}

int main() {
    cout << numTilings(5) << "\n";   // 24
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
