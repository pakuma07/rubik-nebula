# Decode Ways

> Add two terms gated by single/double-digit validity. LC 91 · 🟡 Medium

## Problem
A digit string encodes letters with `A=1 … Z=26`. Count how many ways it decodes (e.g. `"12"` → `"AB"` or `"L"`).

## 🧮 Math / Recurrence
$$
dp[i] = \big[\,s_i \in 1..9\,\big]\cdot dp[i-1] \;+\; \big[\,s_{i-1}s_i \in 10..26\,\big]\cdot dp[i-2]
$$

$$
dp[0] = 1 \ \text{(empty prefix)}
$$

(`[·]` is 1 when the condition holds, else 0.)

## 🧠 Logic
The last character is decoded either **alone** (valid if it is `1..9`, contributing `dp[i−1]`) or **paired** with the previous char (valid if `10..26`, contributing `dp[i−2]`). Leading zeros kill the single-digit option. Add the two compatible counts.

## 🔢 Iteration trace (`s = "226"`)
| i | char | single ok? (`+dp[i-1]`) | pair ok? (`+dp[i-2]`) | dp[i] |
|---|------|-------------------------|------------------------|-------|
| 0 | –    | base | – | 1 |
| 1 | 2    | yes → 1 | – | 1 |
| 2 | 2    | yes → 1 | "22"∈10..26 → +1 | 2 |
| 3 | 6    | yes → 2 | "26"∈10..26 → +1 | **3** |

**Answer = 3** (`"BBF"`, `"BZ"`, `"VF"`).

## 🐍 Python
```python
def num_decodings(s: str) -> int:
    if not s or s[0] == "0":
        return 0
    prev2, prev1 = 1, 1         # dp[i-2], dp[i-1]
    for i in range(1, len(s)):
        cur = 0
        if s[i] != "0":
            cur += prev1
        if 10 <= int(s[i - 1:i + 1]) <= 26:
            cur += prev2
        prev2, prev1 = prev1, cur
    return prev1


if __name__ == "__main__":
    print(num_decodings("226"))   # 3
```

## ⚙️ C++
```cpp
#include <iostream>
#include <string>
using namespace std;

int numDecodings(string s) {
    if (s.empty() || s[0] == '0') return 0;
    int prev2 = 1, prev1 = 1;
    for (size_t i = 1; i < s.size(); ++i) {
        int cur = 0;
        if (s[i] != '0') cur += prev1;
        int pair = (s[i - 1] - '0') * 10 + (s[i] - '0');
        if (pair >= 10 && pair <= 26) cur += prev2;
        prev2 = prev1; prev1 = cur;
    }
    return prev1;
}

int main() {
    cout << numDecodings("226") << "\n";   // 3
}
```

## ⏱️ Complexity
- **Time:** `O(n)`.
- **Space:** `O(1)`.
