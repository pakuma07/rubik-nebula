# Ugly Number II

> Merge three multiple-streams with pointers. LC 264 · 🟡 Medium

## Problem
Find the `n`-th *ugly number* — a positive integer whose only prime factors are `2`, `3`, or `5` (the sequence starts `1, 2, 3, 4, 5, 6, 8, 9, 10, 12, …`).

## 🧮 Math / Recurrence
Every ugly number (after 1) is `2×`, `3×`, or `5×` a smaller ugly number. Maintain three pointers `i2, i3, i5` into the growing list:

$$
dp[k] = \min\big(2 \cdot dp[i2],\ 3 \cdot dp[i3],\ 5 \cdot dp[i5]\big)
$$

then advance every pointer that produced the chosen minimum (to skip duplicates).

## 🧠 Logic
This is a 3-way **merge** of the sorted sequences `2·U`, `3·U`, `5·U`, where `U` is the ugly list itself being built. Each step emits the smallest unseen candidate; advancing all tying pointers keeps the result strictly increasing (e.g. `6 = 2·3 = 3·2` is emitted once).

## 🔢 Iteration trace (first values)
| k | dp | 2·dp[i2] | 3·dp[i3] | 5·dp[i5] | chosen |
|---|----|----------|----------|----------|--------|
| 1 | 1 | 2 | 3 | 5 | 2 |
| 2 | 2 | 4 | 3 | 5 | 3 |
| 3 | 3 | 4 | 6 | 5 | 4 |
| 4 | 4 | 6 | 6 | 5 | 5 |
| 5 | 5 | 6 | 6 | 10 | 6 |

`dp[10] = ` **12**.

## 🐍 Python
```python
def nth_ugly_number(n: int) -> int:
    dp = [1] * n
    i2 = i3 = i5 = 0
    for k in range(1, n):
        nxt = min(2 * dp[i2], 3 * dp[i3], 5 * dp[i5])
        dp[k] = nxt
        if nxt == 2 * dp[i2]: i2 += 1
        if nxt == 3 * dp[i3]: i3 += 1
        if nxt == 5 * dp[i5]: i5 += 1
    return dp[-1]


if __name__ == "__main__":
    print(nth_ugly_number(10))   # 12
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

int nthUglyNumber(int n) {
    vector<int> dp(n, 1);
    int i2 = 0, i3 = 0, i5 = 0;
    for (int k = 1; k < n; ++k) {
        int nxt = min({2 * dp[i2], 3 * dp[i3], 5 * dp[i5]});
        dp[k] = nxt;
        if (nxt == 2 * dp[i2]) ++i2;
        if (nxt == 3 * dp[i3]) ++i3;
        if (nxt == 5 * dp[i5]) ++i5;
    }
    return dp[n - 1];
}

int main() {
    cout << nthUglyNumber(10) << "\n";   // 12
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(n)`.
