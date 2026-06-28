# Edit Distance with Operation Costs

> Levenshtein DP with weighted insert/delete/replace. Variant · 🔴 Hard

## Problem
Like [Edit Distance](09-edit-distance.md), but each operation has its own cost: `ci` (insert), `cd` (delete), `cr` (replace). Minimize the total cost to turn `a` into `b`.

## 🧮 Math / Recurrence
$$
dp[i][j] =
\begin{cases}
dp[i-1][j-1] & a_i = b_j \\
\min\big(c_d + dp[i-1][j],\ c_i + dp[i][j-1],\ c_r + dp[i-1][j-1]\big) & a_i \ne b_j
\end{cases}
$$

Base: `dp[i][0] = i·c_d`, `dp[0][j] = j·c_i`.

## 🧠 Logic
Identical structure to plain edit distance, but each transition is charged its specific weight rather than a flat `1`. Matching is still free. The bases account for deleting an entire prefix of `a` or inserting an entire prefix of `b`. When `c_r > c_i + c_d`, the DP naturally prefers a delete+insert over a replace.

## 🔢 Iteration trace (`a="ab", b="cb"`, `ci=1, cd=1, cr=5`)
- Mismatch `a` vs `c`: replace costs 5, but delete `a` (1) + insert `c` (1) = 2.
- Match `b` ⟹ free. Total = **2**.

## 🐍 Python
```python
def weighted_edit_distance(a: str, b: str, ci: int, cd: int, cr: int) -> int:
    n, m = len(a), len(b)
    dp = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        dp[i][0] = i * cd
    for j in range(1, m + 1):
        dp[0][j] = j * ci
    for i in range(1, n + 1):
        for j in range(1, m + 1):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = min(cd + dp[i - 1][j],
                               ci + dp[i][j - 1],
                               cr + dp[i - 1][j - 1])
    return dp[n][m]


if __name__ == "__main__":
    print(weighted_edit_distance("ab", "cb", 1, 1, 5))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int weightedEditDistance(string a, string b, int ci, int cd, int cr) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    for (int i = 1; i <= n; ++i) dp[i][0] = i * cd;
    for (int j = 1; j <= m; ++j) dp[0][j] = j * ci;
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= m; ++j)
            dp[i][j] = (a[i - 1] == b[j - 1]) ? dp[i - 1][j - 1]
                : min({cd + dp[i - 1][j], ci + dp[i][j - 1], cr + dp[i - 1][j - 1]});
    return dp[n][m];
}

int main() {
    cout << weightedEditDistance("ab", "cb", 1, 1, 5) << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(n·m)` (reducible to `O(m)`).
