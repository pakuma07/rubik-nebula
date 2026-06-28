# Palindrome Partitioning II

> Minimum cuts to split s into palindromes. LC 132 · 🔴 Hard

## Problem
Return the minimum number of cuts needed so every resulting substring of `s` is a palindrome.

## 🧮 Math / Recurrence
Let `cut[i]` = min cuts for prefix `s[:i+1]`, using a palindrome table `pal[j][i]`:

$$
cut[i] =
\begin{cases}
0 & s[0..i]\ \text{palindrome} \\
\min\limits_{0 \le j < i,\ s[j+1..i]\ \text{pal}} \big(cut[j] + 1\big) & \text{otherwise}
\end{cases}
$$

## 🧠 Logic
If the whole prefix is a palindrome, zero cuts. Otherwise try every last palindromic piece `s[j+1..i]`: the cost is the best partition of the earlier prefix `cut[j]` plus one cut. Precompute `pal[][]` so the palindrome test is `O(1)`, making the whole DP `O(n²)`.

## 🔢 Iteration trace (`s = "aab"`)
- `"aa"` palindrome ⟹ `cut[1]=0`. For `i=2` (`b`): last piece `"b"` palindrome, `cut[1]+1 = 1`.
- Answer = **1** ("aa" | "b").

## 🐍 Python
```python
def min_cut(s: str) -> int:
    n = len(s)
    pal = [[False] * n for _ in range(n)]
    cut = [0] * n
    for i in range(n):
        best = i                                  # worst case: i cuts
        for j in range(i + 1):
            if s[j] == s[i] and (i - j < 2 or pal[j + 1][i - 1]):
                pal[j][i] = True
                best = 0 if j == 0 else min(best, cut[j - 1] + 1)
        cut[i] = best
    return cut[n - 1]


if __name__ == "__main__":
    print(min_cut("aab"))   # 1
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int minCut(string s) {
    int n = s.size();
    vector<vector<char>> pal(n, vector<char>(n, false));
    vector<int> cut(n, 0);
    for (int i = 0; i < n; ++i) {
        int best = i;
        for (int j = 0; j <= i; ++j)
            if (s[j] == s[i] && (i - j < 2 || pal[j + 1][i - 1])) {
                pal[j][i] = true;
                best = (j == 0) ? 0 : min(best, cut[j - 1] + 1);
            }
        cut[i] = best;
    }
    return cut[n - 1];
}

int main() {
    cout << minCut("aab") << "\n";   // 1
}
```

## ⏱️ Complexity
- **Time:** `O(n²)`.
- **Space:** `O(n²)`.
