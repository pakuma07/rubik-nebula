# Longest Common Subsequence of Three Strings

> Extend the LCS table to 3D. GFG · 🔴 Hard

## Problem
Return the length of the longest subsequence common to **three** strings `a`, `b`, `c`.

## 🧮 Math / Recurrence
$$
dp[i][j][k] =
\begin{cases}
dp[i-1][j-1][k-1] + 1 & a_i = b_j = c_k \\
\max\big(dp[i-1][j][k],\ dp[i][j-1][k],\ dp[i][j][k-1]\big) & \text{otherwise}
\end{cases}
$$

## 🧠 Logic
Same reasoning as 2-string [LCS](01-longest-common-subsequence.md), one more dimension. A character joins the LCS only when **all three** current characters agree (diagonal `+1` across all axes). Otherwise drop one string's last char and take the best — now three options instead of two.

## 🔢 Iteration trace (`a="geeks", b="geeksfor", c="geeksforgeeks"`)
- Common subsequence `"geeks"` is present in all three.
- `dp[5][8][13] = ` **5**.

## 🐍 Python
```python
def lcs3(a: str, b: str, c: str) -> int:
    n, m, p = len(a), len(b), len(c)
    dp = [[[0] * (p + 1) for _ in range(m + 1)] for _ in range(n + 1)]
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            for k in range(1, p + 1):
                if a[i - 1] == b[j - 1] == c[k - 1]:
                    dp[i][j][k] = dp[i - 1][j - 1][k - 1] + 1
                else:
                    dp[i][j][k] = max(dp[i - 1][j][k], dp[i][j - 1][k], dp[i][j][k - 1])
    return dp[n][m][p]


if __name__ == "__main__":
    print(lcs3("geeks", "geeksfor", "geeksforgeeks"))   # 5
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int lcs3(const string& a, const string& b, const string& c) {
    int n = a.size(), m = b.size(), p = c.size();
    vector<vector<vector<int>>> dp(n + 1,
        vector<vector<int>>(m + 1, vector<int>(p + 1, 0)));
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j)
            for (int k = 1; k <= p; ++k)
                if (a[i - 1] == b[j - 1] && b[j - 1] == c[k - 1])
                    dp[i][j][k] = dp[i - 1][j - 1][k - 1] + 1;
                else
                    dp[i][j][k] = max({dp[i - 1][j][k], dp[i][j - 1][k], dp[i][j][k - 1]});
    return dp[n][m][p];
}

int main() {
    cout << lcs3("geeks", "geeksfor", "geeksforgeeks") << "\n";   // 5
}
```

## ⏱️ Complexity
- **Time:** `O(n·m·p)`.
- **Space:** `O(n·m·p)`.
