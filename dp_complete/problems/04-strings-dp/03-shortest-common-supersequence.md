# Shortest Common Supersequence

> Length n+m−LCS, then reconstruct by walking the table. LC 1092 · 🔴 Hard

## Problem
Return the **shortest string** that has both `a` and `b` as subsequences.

## 🧮 Math / Recurrence
Its length is:

$$
|SCS| = n + m - \text{LCS}(a, b)
$$

Build the LCS table `dp[i][j]`, then walk from `(n,m)` to `(0,0)`: emit matched chars once, and unmatched chars from whichever side the table came from.

## 🧠 Logic
Every common character should appear **once** in the supersequence; the non-common characters of both strings must each appear. Hence the length subtracts the shared LCS. Reconstruction follows the same match/skip decisions used to fill the LCS table, prepending characters as it backtracks.

## 🔢 Iteration trace (`a = "abac"`, `b = "cab"`)
- `LCS = "ab"` (length 2), so `|SCS| = 4 + 3 − 2 = 5`.
- One valid SCS: `"cabac"`.

## 🐍 Python
```python
def shortest_common_supersequence(a: str, b: str) -> str:
    n, m = len(a), len(b)
    dp = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            dp[i][j] = dp[i - 1][j - 1] + 1 if a[i - 1] == b[j - 1] \
                else max(dp[i - 1][j], dp[i][j - 1])

    i, j, res = n, m, []
    while i > 0 and j > 0:
        if a[i - 1] == b[j - 1]:
            res.append(a[i - 1]); i -= 1; j -= 1
        elif dp[i - 1][j] >= dp[i][j - 1]:
            res.append(a[i - 1]); i -= 1
        else:
            res.append(b[j - 1]); j -= 1
    while i > 0:
        res.append(a[i - 1]); i -= 1
    while j > 0:
        res.append(b[j - 1]); j -= 1
    return "".join(reversed(res))


if __name__ == "__main__":
    print(shortest_common_supersequence("abac", "cab"))   # cabac (len 5)
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

string shortestCommonSupersequence(string a, string b) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j)
            dp[i][j] = (a[i - 1] == b[j - 1]) ? dp[i - 1][j - 1] + 1
                                              : max(dp[i - 1][j], dp[i][j - 1]);
    int i = n, j = m; string res;
    while (i > 0 && j > 0) {
        if (a[i - 1] == b[j - 1]) { res += a[--i]; --j; }
        else if (dp[i - 1][j] >= dp[i][j - 1]) res += a[--i];
        else res += b[--j];
    }
    while (i > 0) res += a[--i];
    while (j > 0) res += b[--j];
    reverse(res.begin(), res.end());
    return res;
}

int main() {
    cout << shortestCommonSupersequence("abac", "cab") << "\n";   // cabac
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(n·m)` for reconstruction.
