# Longest Palindromic Subsequence

> LCS of s with its reverse. LC 516 · 🟡 Medium

## Problem
Return the length of the longest **subsequence** of `s` that reads the same forwards and backwards.

## 🧮 Math / Recurrence
Either directly:

$$
dp[i][j] =
\begin{cases}
dp[i+1][j-1] + 2 & s_i = s_j \\
\max(dp[i+1][j],\ dp[i][j-1]) & s_i \ne s_j
\end{cases}
$$

or equivalently `LPS(s) = LCS(s, reverse(s))`.

## 🧠 Logic
A palindromic subsequence reads identically both ways, so it is exactly a subsequence common to `s` and its reverse — hence the LCS identity. The direct interval form pairs matching ends (`+2`) or drops one end, filling `dp[i][j]` over substrings from short to long.

## 🔢 Iteration trace (`s = "bbbab"`)
- Longest palindromic subsequence `"bbbb"` (drop the `a`).
- `dp[0][4] = ` **4**.

## 🐍 Python
```python
def longest_palindrome_subseq(s: str) -> int:
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n - 1, -1, -1):
        dp[i][i] = 1
        for j in range(i + 1, n):
            if s[i] == s[j]:
                dp[i][j] = dp[i + 1][j - 1] + 2
            else:
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    return dp[0][n - 1]


if __name__ == "__main__":
    print(longest_palindrome_subseq("bbbab"))   # 4
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int longestPalindromeSubseq(string s) {
    int n = s.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int i = n - 1; i >= 0; --i) {
        dp[i][i] = 1;
        for (int j = i + 1; j < n; ++j)
            dp[i][j] = (s[i] == s[j]) ? dp[i + 1][j - 1] + 2
                                      : max(dp[i + 1][j], dp[i][j - 1]);
    }
    return dp[0][n - 1];
}

int main() {
    cout << longestPalindromeSubseq("bbbab") << "\n";   // 4
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)`.
