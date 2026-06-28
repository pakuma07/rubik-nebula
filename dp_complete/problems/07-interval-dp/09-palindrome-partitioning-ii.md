# Palindrome Partitioning II

> cut[i] = min(cut[j]+1) over palindromic suffixes. LC 132 · 🔴 Hard

## Problem
Return the minimum number of cuts so every piece of `s` is a palindrome. (Same problem as [04 — Strings DP / Palindrome Partitioning II](../04-strings-dp/22-palindrome-partitioning-ii.md); here framed as a 1D partition DP.)

## 🧮 Math / Recurrence
With a palindrome table `pal[][]`, `cut[i]` = min cuts for prefix `s[:i+1]`:

$$
cut[i] =
\begin{cases}
0 & s[0..i]\ \text{palindrome} \\
\min\limits_{0 \le j < i,\ s[j+1..i]\ \text{pal}} (cut[j] + 1) & \text{otherwise}
\end{cases}
$$

## 🧠 Logic
This is the prefix (1D) flavor of interval DP: the answer for a prefix depends on the best partition of a shorter prefix plus one cut, provided the trailing piece is a palindrome. Precomputing `pal[][]` in `O(n²)` makes each palindrome test `O(1)`.

## 🔢 Iteration trace (`s = "aab"`)
- `"aa"` palindrome ⟹ `cut[1]=0`; `cut[2]=cut[1]+1=1` (piece "b").
- Answer = **1**.

## 🐍 Python
```python
def min_cut(s: str) -> int:
    n = len(s)
    pal = [[False] * n for _ in range(n)]
    cut = [0] * n
    for i in range(n):
        best = i
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
#include <algorithm>
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
