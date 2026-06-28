# Regular Expression Matching

> `.` matches any, `*` matches zero-or-more of the preceding. LC 10 · 🔴 Hard

## Problem
Implement regex matching where `.` matches any single character and `*` matches **zero or more** of the *preceding* element. The pattern must match the **entire** string.

## 🧮 Math / Recurrence
`dp[i][j]` = does `s[:i]` match `p[:j]`:

$$
dp[i][j] =
\begin{cases}
dp[i][j-2] \ \vee\ \big(dp[i-1][j] \wedge (p_{j-1}=s_i \vee p_{j-1}=\text{'.'})\big) & p_j = \text{'*'} \\
dp[i-1][j-1] \wedge (p_j = s_i \vee p_j = \text{'.'}) & \text{otherwise}
\end{cases}
$$

## 🧠 Logic
A `*` with its preceding char is treated two ways: **zero occurrences** (skip the pair, `dp[i][j-2]`) or **one more occurrence** (the preceding char matches `s_i`, so consume `s_i` and stay on the same pattern, `dp[i-1][j]`). A normal char (or `.`) consumes one char from both. The empty-string row seeds `a*`, `a*b*`-style patterns.

## 🔢 Iteration trace (`s = "aab"`, `p = "c*a*b"`)
- `c*` matches zero c's; `a*` matches "aa"; `b` matches "b".
- `dp[3][5] = ` **true**.

## 🐍 Python
```python
def is_match(s: str, p: str) -> bool:
    n, m = len(s), len(p)
    dp = [[False] * (m + 1) for _ in range(n + 1)]
    dp[0][0] = True
    for j in range(1, m + 1):
        if p[j - 1] == "*":
            dp[0][j] = dp[0][j - 2]
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if p[j - 1] == "*":
                dp[i][j] = dp[i][j - 2] or (
                    dp[i - 1][j] and (p[j - 2] == s[i - 1] or p[j - 2] == "."))
            else:
                dp[i][j] = dp[i - 1][j - 1] and (p[j - 1] == s[i - 1] or p[j - 1] == ".")
    return dp[n][m]


if __name__ == "__main__":
    print(is_match("aab", "c*a*b"))   # True
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

bool isMatch(string s, string p) {
    int n = s.size(), m = p.size();
    vector<vector<char>> dp(n + 1, vector<char>(m + 1, false));
    dp[0][0] = true;
    for (int j = 1; j <= m; ++j)
        if (p[j - 1] == '*') dp[0][j] = dp[0][j - 2];
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j) {
            if (p[j - 1] == '*')
                dp[i][j] = dp[i][j - 2] ||
                    (dp[i - 1][j] && (p[j - 2] == s[i - 1] || p[j - 2] == '.'));
            else
                dp[i][j] = dp[i - 1][j - 1] && (p[j - 1] == s[i - 1] || p[j - 1] == '.');
        }
    return dp[n][m];
}

int main() {
    cout << boolalpha << isMatch("aab", "c*a*b") << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(n·m)`.
