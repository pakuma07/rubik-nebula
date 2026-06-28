# Palindromic Substrings (Count)

> Count every palindromic range. LC 647 · 🟡 Medium

## Problem
Count how many **contiguous** substrings of `s` are palindromes (each start/end pair counts separately).

## 🧮 Math / Recurrence
Same boolean table as [Longest Palindromic Substring](19-longest-palindromic-substring.md), summing the `true` cells:

$$
dp[i][j] = (s_i = s_j) \wedge (j - i < 2 \ \vee\ dp[i+1][j-1]), \qquad
\text{answer} = \sum_{i \le j} [\,dp[i][j]\,]
$$

## 🧠 Logic
Every palindromic substring corresponds to one `true` entry of the interval table. Build the table by length and increment a counter for each palindrome found. Expand-around-center (2n−1 centers) is the `O(1)`-space equivalent.

## 🔢 Iteration trace (`s = "aaa"`)
- Length 1: `"a","a","a"` → 3. Length 2: `"aa","aa"` → 2. Length 3: `"aaa"` → 1.
- Total = **6**.

## 🐍 Python
```python
def count_substrings(s: str) -> int:
    n = len(s)
    dp = [[False] * n for _ in range(n)]
    count = 0
    for i in range(n - 1, -1, -1):
        for j in range(i, n):
            if s[i] == s[j] and (j - i < 2 or dp[i + 1][j - 1]):
                dp[i][j] = True
                count += 1
    return count


if __name__ == "__main__":
    print(count_substrings("aaa"))   # 6
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int countSubstrings(string s) {
    int n = s.size(), count = 0;
    vector<vector<char>> dp(n, vector<char>(n, false));
    for (int i = n - 1; i >= 0; --i)
        for (int j = i; j < n; ++j)
            if (s[i] == s[j] && (j - i < 2 || dp[i + 1][j - 1])) {
                dp[i][j] = true;
                ++count;
            }
    return count;
}

int main() {
    cout << countSubstrings("aaa") << "\n";   // 6
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)` (expand-around-center: `O(1)`).
