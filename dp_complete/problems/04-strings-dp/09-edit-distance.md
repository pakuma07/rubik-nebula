# Edit Distance

> Insert / delete / replace — the Levenshtein DP. LC 72 · 🔴 Hard

## Problem
Return the minimum number of single-character **insertions, deletions, or replacements** to turn `a` into `b`.

## 🧮 Math / Recurrence
$$
dp[i][j] =
\begin{cases}
dp[i-1][j-1] & a_i = b_j \\
1 + \min\big(\underbrace{dp[i-1][j]}_{\text{delete}},\ \underbrace{dp[i][j-1]}_{\text{insert}},\ \underbrace{dp[i-1][j-1]}_{\text{replace}}\big) & a_i \ne b_j
\end{cases}
$$

Base: `dp[i][0] = i`, `dp[0][j] = j`.

## 🧠 Logic
Aligning prefixes, the last operation is one of: **match** (free, go diagonal), **replace** (diagonal `+1`), **delete** from `a` (up `+1`), or **insert** into `a` (left `+1`). Taking the cheapest of these covers all edit scripts; the bases handle turning a prefix into the empty string.

## 🔢 Iteration trace (`a = "horse"`, `b = "ros"`)
|     | ε | r | o | s |
|-----|---|---|---|---|
| ε   | 0 | 1 | 2 | 3 |
| h   | 1 | 1 | 2 | 3 |
| o   | 2 | 2 | 1 | 2 |
| r   | 3 | 2 | 2 | 2 |
| s   | 4 | 3 | 3 | 2 |
| e   | 5 | 4 | 4 | **3** |

**Answer = 3** (replace h→r, delete r, delete e).

## 🐍 Python
```python
def edit_distance(a: str, b: str) -> int:
    n, m = len(a), len(b)
    dp = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        dp[i][0] = i
    for j in range(m + 1):
        dp[0][j] = j
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1])
    return dp[n][m]


if __name__ == "__main__":
    print(edit_distance("horse", "ros"))   # 3
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int editDistance(string a, string b) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    for (int i = 0; i <= n; ++i) dp[i][0] = i;
    for (int j = 0; j <= m; ++j) dp[0][j] = j;
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j)
            dp[i][j] = (a[i - 1] == b[j - 1]) ? dp[i - 1][j - 1]
                : 1 + min({dp[i - 1][j], dp[i][j - 1], dp[i - 1][j - 1]});
    return dp[n][m];
}

int main() {
    cout << editDistance("horse", "ros") << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(n·m)` (reducible to `O(m)`).
