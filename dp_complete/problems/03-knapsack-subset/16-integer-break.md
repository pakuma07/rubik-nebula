# Integer Break

> Break n into parts maximizing the product. LC 343 · 🟡 Medium

## Problem
Break integer `n` (with `n ≥ 2`) into **at least two** positive parts and maximize the product of those parts.

## 🧮 Math / Recurrence
$$
dp[i] = \max_{1 \le j < i} \max\big(j \cdot (i - j),\ j \cdot dp[i - j]\big), \qquad dp[1] = 1
$$

## 🧠 Logic
For each first part `j`, the remainder `i − j` is either **left whole** (`j·(i−j)`) or **broken further** (`j·dp[i−j]`). Taking the max over both options and all `j` finds the optimal factorization. (Mathematically the answer favors splitting into 3's, but the DP needs no such insight.)

## 🔢 Iteration trace (`n = 10`)
| i | best | dp[i] |
|---|------|-------|
| 2 | 1·1 | 1 |
| 3 | 1·2 | 2 |
| 4 | 2·2 | 4 |
| … | … | … |
| 10 | 3·3·4? → 3·dp[7] | **36** |

`10 = 3+3+4 → 3·3·4 = 36`.

## 🐍 Python
```python
def integer_break(n: int) -> int:
    dp = [0] * (n + 1)
    dp[1] = 1
    for i in range(2, n + 1):
        for j in range(1, i):
            dp[i] = max(dp[i], j * (i - j), j * dp[i - j])
    return dp[n]


if __name__ == "__main__":
    print(integer_break(10))   # 36
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int integerBreak(int n) {
    vector<int> dp(n + 1, 0);
    dp[1] = 1;
    for (int i = 2; i <= n; ++i)
        for (int j = 1; j < i; ++j)
            dp[i] = max({dp[i], j * (i - j), j * dp[i - j]});
    return dp[n];
}

int main() {
    cout << integerBreak(10) << "\n";   // 36
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n)`.
