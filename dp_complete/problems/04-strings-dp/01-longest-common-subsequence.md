# Longest Common Subsequence

> The canonical 2D string-alignment DP. LC 1143 · 🟡 Medium

## Problem
Return the length of the longest subsequence common to strings `a` and `b` (characters in order, not necessarily contiguous).

## 🧮 Math / Recurrence
$$
dp[i][j] =
\begin{cases}
dp[i-1][j-1] + 1 & a_i = b_j \\
\max(dp[i-1][j],\ dp[i][j-1]) & a_i \ne b_j
\end{cases}
$$

with `dp[0][j] = dp[i][0] = 0`.

## 🧠 Logic
Compare the last characters of the two prefixes. If they **match**, that pair must extend the LCS of the shorter prefixes (diagonal `+1`). If they **differ**, at least one of them is not in the LCS, so drop one side and take the better of up/left. This builds the table bottom-up; `dp[n][m]` is the answer.

## 🔢 Iteration trace (`a = "AGCAT"`, `b = "GAC"`)
|     |  ε | G | A | C |
|-----|----|---|---|---|
| ε   | 0 | 0 | 0 | 0 |
| A   | 0 | 0 | 1 | 1 |
| G   | 0 | 1 | 1 | 1 |
| C   | 0 | 1 | 1 | 2 |
| A   | 0 | 1 | 2 | 2 |
| T   | 0 | 1 | 2 | **2** |

**Answer = 2** (e.g. `"AC"`).

## 🐍 Python
```python
def lcs(a: str, b: str) -> int:
    n, m = len(a), len(b)
    prev = [0] * (m + 1)
    for i in range(1, n + 1):
        cur = [0] * (m + 1)
        for j in range(1, m + 1):
            cur[j] = prev[j - 1] + 1 if a[i - 1] == b[j - 1] else max(prev[j], cur[j - 1])
        prev = cur
    return prev[m]


if __name__ == "__main__":
    print(lcs("AGCAT", "GAC"))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int lcs(const string& a, const string& b) {
    int n = a.size(), m = b.size();
    vector<int> prev(m + 1, 0), cur(m + 1, 0);
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j)
            cur[j] = (a[i - 1] == b[j - 1]) ? prev[j - 1] + 1
                                            : max(prev[j], cur[j - 1]);
        prev = cur;
    }
    return prev[m];
}

int main() {
    cout << lcs("AGCAT", "GAC") << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(m)` with the rolling rows.
