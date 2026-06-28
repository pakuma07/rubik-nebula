# Longest Palindromic Substring

> Interval DP on s[i..j] (or expand-around-center). LC 5 · 🟡 Medium

## Problem
Return the longest **contiguous** substring of `s` that is a palindrome.

## 🧮 Math / Recurrence
$$
dp[i][j] = \big(s_i = s_j\big) \wedge \big(j - i < 2 \ \vee\ dp[i+1][j-1]\big)
$$

Track the longest `(i, j)` with `dp[i][j] = true`. Fill by increasing length.

## 🧠 Logic
A substring `s[i..j]` is a palindrome iff its **ends match** and its **interior** `s[i+1..j-1]` is already a palindrome. Lengths 1 and 2 are base cases. Iterating by length guarantees the inner state is computed first. Expand-around-center achieves the same in `O(1)` space.

## 🔢 Iteration trace (`s = "babad"`)
- Single chars all palindromes. `"bab"`: ends `b==b` and inner `"a"` palindrome ⟹ length 3.
- `"aba"` also length 3.
- Answer = **"bab"** (or "aba").

## 🐍 Python
```python
def longest_palindrome(s: str) -> str:
    n = len(s)
    if n < 2:
        return s
    start, maxlen = 0, 1
    dp = [[False] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = True
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j] and (length == 2 or dp[i + 1][j - 1]):
                dp[i][j] = True
                if length > maxlen:
                    start, maxlen = i, length
    return s[start:start + maxlen]


if __name__ == "__main__":
    print(longest_palindrome("babad"))   # bab
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

string longestPalindrome(string s) {
    int n = s.size();
    if (n < 2) return s;
    int start = 0, maxlen = 1;
    vector<vector<char>> dp(n, vector<char>(n, false));
    for (int i = 0; i < n; ++i) dp[i][i] = true;
    for (int len = 2; len <= n; ++len)
        for (int i = 0; i + len - 1 < n; ++i) {
            int j = i + len - 1;
            if (s[i] == s[j] && (len == 2 || dp[i + 1][j - 1])) {
                dp[i][j] = true;
                if (len > maxlen) { start = i; maxlen = len; }
            }
        }
    return s.substr(start, maxlen);
}

int main() {
    cout << longestPalindrome("babad") << "\n";   // bab
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)` (expand-around-center: `O(1)`).
