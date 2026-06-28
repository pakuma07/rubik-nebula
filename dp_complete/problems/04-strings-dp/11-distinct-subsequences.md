# Distinct Subsequences

> Count how many times t appears as a subsequence of s. LC 115 · 🔴 Hard

## Problem
Count the number of distinct subsequences of `s` that equal `t`.

## 🧮 Math / Recurrence
Let `dp[i][j]` = number of ways the first `i` chars of `s` form the first `j` chars of `t`:

$$
dp[i][j] =
\begin{cases}
dp[i-1][j-1] + dp[i-1][j] & s_i = t_j \\
dp[i-1][j] & s_i \ne t_j
\end{cases}
$$

Base: `dp[i][0] = 1` (empty `t` matches once).

## 🧠 Logic
Consider the last char of `s`. We may **skip** it (`dp[i-1][j]` always available). If it equals `t_j`, we may additionally **use** it to match `t_j`, adding `dp[i-1][j-1]`. Summing the two counts every distinct way; the empty target is matchable exactly one way.

## 🔢 Iteration trace (`s = "rabbbit"`, `t = "rabbit"`)
- The three `b`'s give choices for the two `b`'s of `t`.
- `dp[7][6] = ` **3**.

## 🐍 Python
```python
def num_distinct(s: str, t: str) -> int:
    n, m = len(s), len(t)
    dp = [0] * (m + 1)
    dp[0] = 1
    for i in range(1, n + 1):
        for j in range(m, 0, -1):            # reverse → 1D rolling
            if s[i - 1] == t[j - 1]:
                dp[j] += dp[j - 1]
    return dp[m]


if __name__ == "__main__":
    print(num_distinct("rabbbit", "rabbit"))   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int numDistinct(string s, string t) {
    int n = s.size(), m = t.size();
    vector<unsigned long long> dp(m + 1, 0);
    dp[0] = 1;
    for (int i = 1; i <= n; ++i)
        for (int j = m; j >= 1; --j)         // reverse → 1D rolling
            if (s[i - 1] == t[j - 1]) dp[j] += dp[j - 1];
    return (int)dp[m];
}

int main() {
    cout << numDistinct("rabbbit", "rabbit") << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(m)`.
