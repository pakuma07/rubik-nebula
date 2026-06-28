# Strange Printer

> Interval merge of equal characters. LC 664 · 🔴 Hard

## Problem
A printer prints sequences of one character, each print covering a contiguous block and overwriting existing characters. Return the minimum number of prints to produce string `s`.

## 🧮 Math / Recurrence
`dp[i][j]` = min prints for `s[i..j]`:

$$
dp[i][j] = \min\Big(dp[i][j-1] + 1,\ \min_{\substack{i \le k < j \\ s_k = s_j}}\big(dp[i][k] + dp[k+1][j-1]\big)\Big)
$$

## 🧠 Logic
Start by printing `s[i]` across the whole range (cost contributes via `dp[i][j-1]+1`). The saving comes when some earlier position `k` has the **same** character as `s[j]`: that one stroke can be extended to cover `s[j]` too, merging the intervals and removing a separate print for `s[j]`. Minimizing over such `k` yields the recurrence.

## 🔢 Iteration trace (`s = "aba"`)
- Print "aaa" then "b" → 2 prints. Answer = **2**.

## 🐍 Python
```python
from functools import lru_cache

def strange_printer(s: str) -> int:
    if not s:
        return 0

    @lru_cache(maxsize=None)
    def dp(i: int, j: int) -> int:
        if i > j:
            return 0
        best = dp(i, j - 1) + 1
        for k in range(i, j):
            if s[k] == s[j]:
                best = min(best, dp(i, k) + dp(k + 1, j - 1))
        return best

    return dp(0, len(s) - 1)


if __name__ == "__main__":
    print(strange_printer("aba"))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

vector<vector<int>> memo;

int dp(const string& s, int i, int j) {
    if (i > j) return 0;
    if (memo[i][j]) return memo[i][j];
    int best = dp(s, i, j - 1) + 1;
    for (int k = i; k < j; ++k)
        if (s[k] == s[j])
            best = min(best, dp(s, i, k) + dp(s, k + 1, j - 1));
    return memo[i][j] = best;
}

int strangePrinter(string s) {
    if (s.empty()) return 0;
    int n = s.size();
    memo.assign(n, vector<int>(n, 0));
    return dp(s, 0, n - 1);
}

int main() {
    cout << strangePrinter("aba") << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n³)`.
- **Space:** `O(n²)`.
