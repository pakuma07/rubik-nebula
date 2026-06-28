# Delete Operation for Two Strings

> Minimum deletions = n + m − 2·LCS. LC 583 · 🟡 Medium

## Problem
In one step you may delete a character from either string. Return the minimum steps to make `a` and `b` equal.

## 🧮 Math / Recurrence
$$
\text{ans} = n + m - 2 \cdot \text{LCS}(a, b)
$$

## 🧠 Logic
The characters you **keep** must form a common subsequence, and the optimal choice keeps the **longest** one. Everything else in both strings is deleted: `n − LCS` from `a` and `m − LCS` from `b`. Summing gives `n + m − 2·LCS`.

## 🔢 Iteration trace (`a = "sea"`, `b = "eat"`)
- `LCS("sea","eat") = "ea"` (length 2).
- ans = `3 + 3 − 2·2 = ` **2** (delete `'s'` from "sea", `'t'` from "eat").

## 🐍 Python
```python
def min_distance(a: str, b: str) -> int:
    n, m = len(a), len(b)
    prev = [0] * (m + 1)
    for i in range(1, n + 1):
        cur = [0] * (m + 1)
        for j in range(1, m + 1):
            cur[j] = prev[j - 1] + 1 if a[i - 1] == b[j - 1] else max(prev[j], cur[j - 1])
        prev = cur
    lcs = prev[m]
    return n + m - 2 * lcs


if __name__ == "__main__":
    print(min_distance("sea", "eat"))   # 2
```

## ⚙️ C++
```cpp
#include <algorithm>
#include <iostream>
#include <string>
#include <vector>
using namespace std;

int minDistance(string a, string b) {
    int n = a.size(), m = b.size();
    vector<int> prev(m + 1, 0), cur(m + 1, 0);
    for (int i = 1; i <= n; ++i) {
        for (int j = 1; j <= m; ++j)
            cur[j] = (a[i - 1] == b[j - 1]) ? prev[j - 1] + 1
                                            : max(prev[j], cur[j - 1]);
        prev = cur;
    }
    return n + m - 2 * prev[m];
}

int main() {
    cout << minDistance("sea", "eat") << "\n";   // 2
}
```

## ⏱️ Complexity
- **Time:** `O(n·m)`.
- **Space:** `O(m)`.
