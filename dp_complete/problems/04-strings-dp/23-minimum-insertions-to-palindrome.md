# Minimum Insertions to Make a Palindrome

> Answer = n − Longest Palindromic Subsequence. LC 1312 · 🔴 Hard

## Problem
Return the minimum number of characters to **insert** anywhere in `s` to make it a palindrome.

## 🧮 Math / Recurrence
$$
\text{answer} = n - \text{LPS}(s)
$$

or directly with an interval DP counting insertions:

$$
dp[i][j] =
\begin{cases}
dp[i+1][j-1] & s_i = s_j \\
1 + \min(dp[i+1][j],\ dp[i][j-1]) & s_i \ne s_j
\end{cases}
$$

## 🧠 Logic
Characters belonging to the longest palindromic subsequence are already "free"; every other character needs a mirror inserted, so the count is `n − LPS`. Equivalently, the interval DP keeps matching ends for free and pays one insertion to mirror an unmatched end.

## 🔢 Iteration trace (`s = "leetcode"`)
- LPS = `"eee"` (length 3). Insertions = `8 − 3 = ` **5**.

## 🐍 Python
```python
def min_insertions(s: str) -> int:
    n = len(s)
    dp = [[0] * n for _ in range(n)]
    for i in range(n - 1, -1, -1):
        for j in range(i + 1, n):
            if s[i] == s[j]:
                dp[i][j] = dp[i + 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i + 1][j], dp[i][j - 1])
    return dp[0][n - 1]


if __name__ == "__main__":
    print(min_insertions("leetcode"))   # 5
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int minInsertions(string s) {
    int n = s.size();
    vector<vector<int>> dp(n, vector<int>(n, 0));
    for (int i = n - 1; i >= 0; --i)
        for (int j = i + 1; j < n; ++j)
            dp[i][j] = (s[i] == s[j]) ? dp[i + 1][j - 1]
                                      : 1 + min(dp[i + 1][j], dp[i][j - 1]);
    return dp[0][n - 1];
}

int main() {
    cout << minInsertions("leetcode") << "\n";   // 5
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)`.
