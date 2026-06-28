# Minimum Window Subsequence

> Find the shortest window of S containing T as a subsequence. LC 727 · 🔴 Hard

## Problem
Return the **shortest** substring (window) of `S` such that `T` is a subsequence of it. Ties broken by smallest start index; `""` if none.

## 🧮 Math / Recurrence
`dp[i][j]` = the **start index** in `S` of the shortest window ending at `i` that matches `T[:j]`:

$$
dp[i][j] =
\begin{cases}
dp[i-1][j-1] & S_{i-1} = T_{j-1} \\
dp[i-1][j] & \text{otherwise}
\end{cases}
$$

Track the best `(length, start)` whenever `dp[i][m]` is set.

## 🧠 Logic
We greedily extend a match: when characters agree, the window's start is inherited from the shorter match `dp[i-1][j-1]`; otherwise the current char is just absorbed, keeping the same start `dp[i-1][j]`. Each time `T` is fully matched (`j = m`), we know the window `[start, i)` and keep the shortest.

## 🔢 Iteration trace (`S = "abcdebdde"`, `T = "bde"`)
- Window `"bcde"` (indices 1–4) matches `b,d,e` → length 4.
- Window `"bdde"` (indices 5–8) also matches → length 4, but later start.
- Answer = **"bcde"**.

## 🐍 Python
```python
def min_window(S: str, T: str) -> str:
    n, m = len(S), len(T)
    INF = float("inf")
    # dp[j] = start index of shortest window matching T[:j] ending at current i
    dp = [0] + [INF] * m
    best_len, best_start = INF, -1
    for i in range(1, n + 1):
        for j in range(m, 0, -1):
            if S[i - 1] == T[j - 1]:
                dp[j] = dp[j - 1]
        if dp[m] != INF:
            if i - dp[m] < best_len:
                best_len = i - dp[m]
                best_start = dp[m]
        dp[0] = i                            # reset start for new window
    return "" if best_start < 0 else S[best_start:best_start + best_len]


if __name__ == "__main__":
    print(min_window("abcdebdde", "bde"))   # bcde
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

string minWindow(string S, string T) {
    int n = S.size(), m = T.size();
    const int INF = 1e9;
    vector<int> dp(m + 1, INF);
    dp[0] = 0;
    int bestLen = INF, bestStart = -1;
    for (int i = 1; i <= n; ++i) {
        for (int j = m; j >= 1; --j)
            if (S[i - 1] == T[j - 1]) dp[j] = dp[j - 1];
        if (dp[m] != INF && i - dp[m] < bestLen) {
            bestLen = i - dp[m];
            bestStart = dp[m];
        }
        dp[0] = i;
    }
    return bestStart < 0 ? "" : S.substr(bestStart, bestLen);
}

int main() {
    cout << minWindow("abcdebdde", "bde") << "\n";   // bcde
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(m)`.
