# Interleaving String

> dp[i][j]: can s3 be built by interleaving s1, s2. LC 97 · 🔴 Hard

## Problem
Decide whether `s3` is formed by **interleaving** `s1` and `s2` (preserving each one's order).

## 🧮 Math / Recurrence
Requires `len(s3) = len(s1) + len(s2)`. Let `dp[i][j]` = "first `i+j` chars of `s3` use `i` of `s1` and `j` of `s2`":

$$
dp[i][j] = \big(dp[i-1][j] \wedge s1_i = s3_{i+j}\big) \ \vee\ \big(dp[i][j-1] \wedge s2_j = s3_{i+j}\big)
$$

## 🧠 Logic
The character at position `i+j` of `s3` came from either `s1`'s `i`-th char (then `dp[i-1][j]` must hold) or `s2`'s `j`-th char (then `dp[i][j-1]`). OR-ing the two sources tests every interleaving without enumerating them. The length check rules out impossible cases instantly.

## 🔢 Iteration trace (`s1="aab", s2="axy", s3="aaxaby"`)
- Length `3+3 = 6` ✓.
- A valid interleave: `a(s1) a(s1) x(s2) a... ` — DP reaches `dp[3][3] = ` **true**.

## 🐍 Python
```python
def is_interleave(s1: str, s2: str, s3: str) -> bool:
    n, m = len(s1), len(s2)
    if n + m != len(s3):
        return False
    dp = [[False] * (m + 1) for _ in range(n + 1)]
    dp[0][0] = True
    for i in range(n + 1):
        for j in range(m + 1):
            if i > 0 and s1[i - 1] == s3[i + j - 1]:
                dp[i][j] = dp[i][j] or dp[i - 1][j]
            if j > 0 and s2[j - 1] == s3[i + j - 1]:
                dp[i][j] = dp[i][j] or dp[i][j - 1]
    return dp[n][m]


if __name__ == "__main__":
    print(is_interleave("aab", "axy", "aaxaby"))   # True
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

bool isInterleave(string s1, string s2, string s3) {
    int n = s1.size(), m = s2.size();
    if (n + m != (int)s3.size()) return false;
    vector<vector<char>> dp(n + 1, vector<char>(m + 1, false));
    dp[0][0] = true;
    for (int i = 0; i <= n; ++i)
        for (int j = 0; j <= m; ++j) {
            if (i > 0 && s1[i - 1] == s3[i + j - 1]) dp[i][j] = dp[i][j] || dp[i - 1][j];
            if (j > 0 && s2[j - 1] == s3[i + j - 1]) dp[i][j] = dp[i][j] || dp[i][j - 1];
        }
    return dp[n][m];
}

int main() {
    cout << boolalpha << isInterleave("aab", "axy", "aaxaby") << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(n·m)` (reducible to `O(m)`).
