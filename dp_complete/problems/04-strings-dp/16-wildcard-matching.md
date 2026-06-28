# Wildcard Matching

> `?` matches one char, `*` matches any sequence. LC 44 · 🔴 Hard

## Problem
Match `s` against pattern `p` where `?` matches any single character and `*` matches **any sequence** (including empty). Must match the whole string.

## 🧮 Math / Recurrence
$$
dp[i][j] =
\begin{cases}
dp[i-1][j] \ \vee\ dp[i][j-1] & p_j = \text{'*'} \\
dp[i-1][j-1] & p_j = s_i \ \text{or}\ p_j = \text{'?'} \\
\text{false} & \text{otherwise}
\end{cases}
$$

## 🧠 Logic
The `*` is the only tricky token: it can **absorb one more character** of `s` (`dp[i-1][j]`, star stays) or **match empty** (`dp[i][j-1]`, move past star). A `?` or exact char consumes one char from each side. The empty-string row stays true only while the pattern is a run of `*`.

## 🔢 Iteration trace (`s = "adceb"`, `p = "*a*b"`)
- First `*` matches empty; `a` matches; second `*` matches `"dce"`; `b` matches.
- `dp[5][4] = ` **true**.

## 🐍 Python
```python
def is_match(s: str, p: str) -> bool:
    n, m = len(s), len(p)
    dp = [[False] * (m + 1) for _ in range(n + 1)]
    dp[0][0] = True
    for j in range(1, m + 1):
        if p[j - 1] == "*":
            dp[0][j] = dp[0][j - 1]
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if p[j - 1] == "*":
                dp[i][j] = dp[i - 1][j] or dp[i][j - 1]
            elif p[j - 1] == "?" or p[j - 1] == s[i - 1]:
                dp[i][j] = dp[i - 1][j - 1]
    return dp[n][m]


if __name__ == "__main__":
    print(is_match("adceb", "*a*b"))   # True
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
        if (p[j - 1] == '*') dp[0][j] = dp[0][j - 1];
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j) {
            if (p[j - 1] == '*') dp[i][j] = dp[i - 1][j] || dp[i][j - 1];
            else if (p[j - 1] == '?' || p[j - 1] == s[i - 1]) dp[i][j] = dp[i - 1][j - 1];
        }
    return dp[n][m];
}

int main() {
    cout << boolalpha << isMatch("adceb", "*a*b") << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(n·m)` (reducible to `O(m)`).
