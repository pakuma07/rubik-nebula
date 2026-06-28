# Valid Palindrome III

> Removable up to k chars ⟺ n − LPS ≤ k. LC 1216 · 🔴 Hard

## Problem
Return whether `s` can be turned into a palindrome by **removing at most `k`** characters.

## 🧮 Math / Recurrence
Let `dp[i][j]` be the minimum deletions to make `s[i..j]` a palindrome (equivalently `len − LPS`):

$$
dp[i][j] =
\begin{cases}
dp[i+1][j-1] & s_i = s_j \\
1 + \min(dp[i+1][j],\ dp[i][j-1]) & s_i \ne s_j
\end{cases}
\qquad \text{answer} = \big(dp[0][n-1] \le k\big)
$$

## 🧠 Logic
Removing characters to form a palindrome keeps exactly a palindromic subsequence; the cheapest is to keep the **longest** one, so minimum deletions `= n − LPS`. The interval DP computes that deletion count directly; compare against `k`.

## 🔢 Iteration trace (`s = "abcdeca"`, `k = 2`)
- LPS = `"acdca"`/`"aceca"` length 5. Deletions = `7 − 5 = 2 ≤ 2` ⟹ **true**.

## 🐍 Python
```python
def is_valid_palindrome(s: str, k: int) -> bool:
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n - 1, -1, -1):
        for j in range(i + 1, n):
            if s[i] == s[j]:
                dp[i][j] = dp[i + 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i + 1][j], dp[i][j - 1])
    return dp[0][n - 1] <= k


if __name__ == "__main__":
    print(is_valid_palindrome("abcdeca", 2))   # True
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

bool isValidPalindrome(string s, int k) {
    int n = s.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int i = n - 1; i >= 0; --i)
        for (int j = i + 1; j < n; ++j)
            dp[i][j] = (s[i] == s[j]) ? dp[i + 1][j - 1]
                                      : 1 + min(dp[i + 1][j], dp[i][j - 1]);
    return dp[0][n - 1] <= k;
}

int main() {
    cout << boolalpha << isValidPalindrome("abcdeca", 2) << "\n";   // true
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)`.
