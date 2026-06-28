# Minimum ASCII Delete Sum for Two Strings

> LCS weighted by ASCII value. LC 712 · 🟡 Medium

## Problem
Delete characters (each costing its ASCII value) to make `a` and `b` equal. Minimize the total deleted ASCII sum.

## 🧮 Math / Recurrence
Let `dp[i][j]` = min ASCII delete cost to equalize `a[i:]` and `b[j:]`:

$$
dp[i][j] =
\begin{cases}
dp[i+1][j+1] & a_i = b_j \\
\min\big(\text{ord}(a_i) + dp[i+1][j],\ \text{ord}(b_j) + dp[i][j+1]\big) & a_i \ne b_j
\end{cases}
$$

Base: emptying a suffix costs the sum of its ASCII values.

## 🧠 Logic
This is the weighted twin of [Delete Operation](05-delete-operation-two-strings.md): instead of counting deletions we **charge by ASCII**. Matching characters cost nothing and advance both; on a mismatch, delete the cheaper of the two leading characters and recurse. Equivalently, maximize the ASCII sum of the kept common subsequence.

## 🔢 Iteration trace (`a = "sea"`, `b = "eat"`)
- Keep `"ea"`. Delete `'s'`(115) and `'t'`(116).
- Total = `115 + 116 = ` **231**.

## 🐍 Python
```python
def minimum_delete_sum(a: str, b: str) -> int:
    n, m = len(a), len(b)
    dp = [[0] * (m + 1) for _ in range(n + 1)]
    for i in range(n - 1, -1, -1):
        dp[i][m] = dp[i + 1][m] + ord(a[i])
    for j in range(m - 1, -1, -1):
        dp[n][j] = dp[n][j + 1] + ord(b[j])
    for i in range(n - 1, -1, -1):
        for j in range(m - 1, -1, -1):
            if a[i] == b[j]:
                dp[i][j] = dp[i + 1][j + 1]
            else:
                dp[i][j] = min(ord(a[i]) + dp[i + 1][j], ord(b[j]) + dp[i][j + 1])
    return dp[0][0]


if __name__ == "__main__":
    print(minimum_delete_sum("sea", "eat"))   # 231
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int minimumDeleteSum(string a, string b) {
    int n = a.size(), m = b.size();
    vector<vector<int>> dp(n + 1, vector<int>(m + 1, 0));
    for (int i = n - 1; i >= 0; --i) dp[i][m] = dp[i + 1][m] + a[i];
    for (int j = m - 1; j >= 0; --j) dp[n][j] = dp[n][j + 1] + b[j];
    for (int i = n - 1; i >= 0; --i)
        for (int j = m - 1; j >= 0; --j)
            dp[i][j] = (a[i] == b[j]) ? dp[i + 1][j + 1]
                : min(a[i] + dp[i + 1][j], b[j] + dp[i][j + 1]);
    return dp[0][0];
}

int main() {
    cout << minimumDeleteSum("sea", "eat") << "\n";   // 231
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(n·m)`.
