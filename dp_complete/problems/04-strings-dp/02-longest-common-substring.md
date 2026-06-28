# Longest Common Substring

> Like LCS, but mismatches reset to 0 (contiguity). GFG · 🟡 Medium

## Problem
Return the length of the longest **contiguous** substring common to `a` and `b`.

## 🧮 Math / Recurrence
$$
dp[i][j] =
\begin{cases}
dp[i-1][j-1] + 1 & a_i = b_j \\
0 & a_i \ne b_j
\end{cases}
$$

Answer = `max_{i,j} dp[i][j]` (not `dp[n][m]`).

## 🧠 Logic
A substring must be unbroken, so a mismatch **terminates** the current run — reset the cell to 0 instead of inheriting neighbors (the key difference from [LCS](01-longest-common-subsequence.md)). Each `dp[i][j]` is the length of the common suffix ending at `a[i-1], b[j-1]`; the global max over the table is the answer.

## 🔢 Iteration trace (`a = "ABCDE"`, `b = "ABFCE"`)
- Diagonal runs: `AB` gives length 2 at `(2,2)`; isolated `C`, `E` give 1.
- Max cell = **2** (`"AB"`).

## 🐍 Python
```python
def longest_common_substring(a: str, b: str) -> int:
    n, m = len(a), len(b)
    prev = [0] * (m + 1)
    best = 0
    for i in range(1, n + 1):
        cur = [0] * (m + 1)
        for j in range(1, m + 1):
            if a[i - 1] == b[j - 1]:
                cur[j] = prev[j - 1] + 1
                best = max(best, cur[j])
        prev = cur
    return best


if __name__ == "__main__":
    print(longest_common_substring("ABCDE", "ABFCE"))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int longestCommonSubstring(const string& a, const string& b) {
    int n = a.size(), m = b.size(), best = 0;
    vector<int> prev(m + 1, 0), cur(m + 1, 0);
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j) {
            cur[j] = (a[i - 1] == b[j - 1]) ? prev[j - 1] + 1 : 0;
            best = max(best, cur[j]);
        }
        prev = cur;
    }
    return best;
}

int main() {
    cout << longestCommonSubstring("ABCDE", "ABFCE") << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(m)`.
