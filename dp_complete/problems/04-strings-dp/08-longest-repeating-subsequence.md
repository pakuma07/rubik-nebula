# Longest Repeating Subsequence

> LCS(s, s) forbidding the diagonal (i ≠ j). GFG · 🟡 Medium

## Problem
Find the length of the longest subsequence that appears **at least twice** in `s`, using different index positions for the two occurrences.

## 🧮 Math / Recurrence
Run LCS of `s` with itself, but a match counts only when the indices differ:

$$
dp[i][j] =
\begin{cases}
dp[i-1][j-1] + 1 & s_i = s_j \ \textbf{and}\ i \ne j \\
\max(dp[i-1][j],\ dp[i][j-1]) & \text{otherwise}
\end{cases}
$$

## 🧠 Logic
A repeating subsequence is a subsequence common to `s` and `s` that uses **two distinct alignments** of each character. Forbidding `i == j` prevents pairing a character with itself, so the two found copies occupy different positions — exactly a repeated subsequence.

## 🔢 Iteration trace (`s = "aabb"`)
- `'a'` repeats (positions 0,1), `'b'` repeats (2,3) ⟹ `"ab"` appears twice.
- `dp[4][4] = ` **2**.

## 🐍 Python
```python
def longest_repeating_subsequence(s: str) -> int:
    n = len(s)
    dp = [[0] * (n + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for j in range(1, n + 1):
            if s[i - 1] == s[j - 1] and i != j:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[n][n]


if __name__ == "__main__":
    print(longest_repeating_subsequence("aabb"))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int longestRepeatingSubsequence(const string& s) {
    int n = s.size();
    vector<vector<int>> dp(n + 1, vector<int>(n + 1, 0));
    for (int i = 1; i <= n; ++i)
        for (int j = 1; j <= n; ++j)
            dp[i][j] = (s[i - 1] == s[j - 1] && i != j)
                ? dp[i - 1][j - 1] + 1
                : max(dp[i - 1][j], dp[i][j - 1]);
    return dp[n][n];
}

int main() {
    cout << longestRepeatingSubsequence("aabb") << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)`.
