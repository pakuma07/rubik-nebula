# Count Different Palindromic Subsequences

> Interval DP with boundary-char dedup. LC 730 · 🔴 Hard

## Problem
Count the number of **distinct** non-empty palindromic subsequences of `s`, modulo `10^9 + 7`.

## 🧮 Math / Recurrence
`dp[i][j]` = distinct palindromic subsequences in `s[i..j]`. When `s_i = s_j = c`, locate the **innermost** matching `c`'s at positions `lo, hi`:

$$
dp[i][j] =
\begin{cases}
2\,dp[i+1][j-1] + 2 & \text{no inner } c \\
2\,dp[i+1][j-1] + 1 & \text{exactly one inner } c \\
2\,dp[i+1][j-1] - dp[lo+1][hi-1] & \ge 2 \text{ inner } c
\end{cases}
$$

and when `s_i \ne s_j`: `dp[i][j] = dp[i+1][j] + dp[i][j-1] - dp[i+1][j-1]`.

## 🧠 Logic
When the ends differ, inclusion–exclusion merges the two sub-intervals and removes the doubly-counted middle. When the ends match on char `c`, every palindrome of the interior can be wrapped by `c…c` (doubling), plus the singletons `c` and `cc`; if `c` already appears inside, those wrapped forms were **already counted**, so we subtract the inner-duplicate region to keep subsequences *distinct*.

## 🔢 Iteration trace (`s = "bccb"`)
- Distinct palindromic subsequences: `b, c, bb, cc, bcb, bccb` → **6**.

## 🐍 Python
```python
def count_palindromic_subsequences(s: str) -> int:
    MOD = 10**9 + 7
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = 1
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                lo, hi = i + 1, j - 1
                while lo <= hi and s[lo] != s[i]:
                    lo += 1
                while lo <= hi and s[hi] != s[i]:
                    hi -= 1
                if lo > hi:
                    dp[i][j] = 2 * dp[i + 1][j - 1] + 2
                elif lo == hi:
                    dp[i][j] = 2 * dp[i + 1][j - 1] + 1
                else:
                    dp[i][j] = 2 * dp[i + 1][j - 1] - dp[lo + 1][hi - 1]
            else:
                dp[i][j] = dp[i + 1][j] + dp[i][j - 1] - dp[i + 1][j - 1]
            dp[i][j] %= MOD
    return dp[0][n - 1]


if __name__ == "__main__":
    print(count_palindromic_subsequences("bccb"))   # 6
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;
const long long MOD = 1e9 + 7;

int countPalindromicSubsequences(string s) {
    int n = s.size();
    vector<vector<long long>> dp(n, vector<long long>(n, 0));
    for (int i = 0; i < n; ++i) dp[i][i] = 1;
    for (int len = 2; len <= n; ++len)
        for (int i = 0; i + len - 1 < n; ++i) {
            int j = i + len - 1;
            if (s[i] == s[j]) {
                int lo = i + 1, hi = j - 1;
                while (lo <= hi && s[lo] != s[i]) ++lo;
                while (lo <= hi && s[hi] != s[i]) --hi;
                if (lo > hi)       dp[i][j] = 2 * dp[i + 1][j - 1] + 2;
                else if (lo == hi) dp[i][j] = 2 * dp[i + 1][j - 1] + 1;
                else               dp[i][j] = 2 * dp[i + 1][j - 1] - dp[lo + 1][hi - 1];
            } else {
                dp[i][j] = dp[i + 1][j] + dp[i][j - 1] - dp[i + 1][j - 1];
            }
            dp[i][j] = ((dp[i][j] % MOD) + MOD) % MOD;
        }
    return (int)dp[0][n - 1];
}

int main() {
    cout << countPalindromicSubsequences("bccb") << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)`.
